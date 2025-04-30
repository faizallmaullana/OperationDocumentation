# Wireguard API

Simple Architecture

```plaintext
[EC2 Client]
  |
  | (POST public key)
  v
[API Server (di WireGuard Server)]
  |
  | (wg set wg0 peer)
  v
[WireGuard Server]
```

## Server Configuration

Buat config ```/etc/wireguard/wg0.conf``` minimal:

```conf
[Interface]
PrivateKey = <SERVER_PRIVATE_KEY>
Address = 10.8.0.1/24
ListenPort = 51820
```

### Start

```sh
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```

## Simple API Server

```python
from fastapi import FastAPI
import subprocess
from pydantic import BaseModel

app = FastAPI()

IP_BASE = "10.8.0."
current_ip_index = 2  # mulai dari 10.8.0.2

class Peer(BaseModel):
    public_key: str

peers = {}

@app.post("/register")
def register(peer: Peer):
    global current_ip_index

    assigned_ip = f"{IP_BASE}{current_ip_index}/32"
    current_ip_index += 1

    # Simpan
    peers[peer.public_key] = assigned_ip

    # Tambah ke WireGuard
    cmd = [
        "wg", "set", "wg0",
        "peer", peer.public_key,
        "allowed-ips", assigned_ip
    ]
    subprocess.run(cmd, check=True)

    return {"assigned_ip": assigned_ip}
```

```python
@app.post("/deregister")
def deregister(peer: Peer):
    if peer.public_key in peers:
        cmd = [
            "wg", "set", "wg0",
            "peer", peer.public_key,
            "remove"
        ]
        subprocess.run(cmd, check=True)
        del peers[peer.public_key]
        return {"status": "removed"}
    return {"status": "not found"}
```

### EC2 script

```sh
#!/bin/bash
apt update
apt install wireguard curl -y

# Generate key
PRIVATE_KEY=$(wg genkey)
PUBLIC_KEY=$(echo $PRIVATE_KEY | wg pubkey)

# Daftar ke server
ASSIGNED_IP=$(curl -s -X POST http://<SERVER_PUBLIC_IP>:8000/register -H "Content-Type: application/json" -d "{\"public_key\": \"$PUBLIC_KEY\"}" | jq -r '.assigned_ip')

# Tulis config
cat > /etc/wireguard/wg0.conf <<EOF
[Interface]
PrivateKey = $PRIVATE_KEY
Address = $ASSIGNED_IP

[Peer]
PublicKey = <SERVER_PUBLIC_KEY>
Endpoint = <SERVER_PUBLIC_IP>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
EOF

# Start WireGuard
systemctl enable wg-quick@wg0
systemctl start wg-quick@wg0
```