# Swap Memory Simple

- [Home](../README.md)
- [Swap Full](SwapMemory.md)
- [Simple Method](SimpleMethod.md)

Oversimplifly swap method

```sh
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

### Make it Permanent

```sh
sudo nano /etc/fstab
```

than add this line

```
/swapfile swap swap defaults 0 0
```