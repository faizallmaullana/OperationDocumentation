# Swap Memory

[Home](../README.md)

Swap memory here is usefull when using server with low memory, you can alternatively increase memory using this following command

## 1 Cek Swap yang ada

```sh
sudo swapon --show
free -h
```

## 2. Buat Swap File

```sh
sudo fallocate -l 2G /swapfile

```

If fallocate not available, use:

```sh
sudo chmod 600 /swapfile
ls -lh /swapfile
```

## 3. Atur Izin Swap File

```sh
sudo chmod 600 /swapfile
ls -lh /swapfile
```

##  4. Format sebagai Swap

```sh
sudo mkswap /swapfile
```

## 5. Aktifkan Swap

```sh
sudo swapon /swapfile
```

## 6. Verifikasi

```sh
sudo swapon --show
free -h
```

## 7. Agar Permanen (Setelah Reboot)

Edit file ```/etc/fstab```:

```sh
sudo nano /etc/fstab
```

Add this follwing line:

```sh
/swapfile swap swap defaults 0 0
```

## 8. Atur Swappiness (Opsional)

Swappiness controls how often the system uses swap (value 0-100). The default value is usually 60, which can be changed to 10 for example to prioritize RAM:

```sh
sudo sysctl vm.swappiness=10
```

Make it permanent, edit on ```/etc/sysctl.conf```:

```sh
sudo nano /etc/sysctl.conf
```

Add:

```ini
vm.swappiness=10
```

## 9. Hapus Swap (Opsional)

if it is no longer needed:

```sh
sudo swapoff /swapfile
sudo rm /swapfile
```

Delete following line from ```/etc/fstab```.