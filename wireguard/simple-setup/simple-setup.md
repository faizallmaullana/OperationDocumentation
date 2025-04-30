# Simple Setup Wireguard

## Instalation

Do this on both server and client

```sh
sudo apt update
sudo apt install wireguard
```

### Generate Key
```sh
wg genkey | tee privatekey | wg pubkey > publickey
```

Now I have
- ```privateKey``` -> Keep Secret
- ```publicKey``` -> To share to other macine

## Server Configuration

Create ```/etc/wireguard/wg0.conf```

```conf
[Interface]
PrivateKey = SERVER_PRIVATE_KEY
Address = 10.0.0.1/24
ListenPort = 51820
SaveConfig = true

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.2/32
```

## Client Configuration

Create ```/etc/wireguard/wg0.conf```

```conf
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.2/24

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_PUBLIC_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

## Start Wireguard

### Start

```sh
sudo wg-quick up wg0
```

### Stop

```sh
sudo wg-quick down wg0
```

## Set Autostart Wireguard

Optional

```sh
sudo systemctl enable wg-quick@wg0
```