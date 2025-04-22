# Instalation K3s

- [Home](../README.md)
- [Instalation](Instalation.md)
- [Longhorn](Longhorn.md)

## Master

```sh
curl -sfL https://get.k3s.io | sh -
```

## Worker

Get Master token from master node

```sh
sudo cat /var/lib/rancher/k3s/server/node-token
```

Than copy master token and the ip of master here

```sh
curl -sfL https://get.k3s.io | K3S_URL=https://<K3S-MASTER-IP>:6443 K3S_TOKEN=<YOUR_K3S_TOKEN> sh -
```

### Connecting to master

On master, copy all from this file

```sh
sudo cat /etc/rancher/k3s/k3s.yaml
```

Than paste to worker node to ```~/.kube/config```

On ~/.kube/config file, follow this rule:

- Change ```server: 127.0.0.1:6443``` to ```[ip_master]:6443```
- Remove section ```certificate-authority-data: ...```
- Change following removed section to ```insecure-skip-tls-verify: true```
