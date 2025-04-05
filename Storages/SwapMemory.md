# Swap Memory

[Home](../README.md)

Swap memory here is usefull when using server with low memory, you can alternatively increase memory using this following command

## 1 Check existing Swap

```sh
sudo swapon --show
free -h
```

## 2. Create Swap File

```sh
sudo fallocate -l 2G /swapfile
```

If fallocate not available, use:

```sh
sudo chmod 600 /swapfile
ls -lh /swapfile
```

## 3. Set Swap File Permissions

```sh
sudo chmod 600 /swapfile
ls -lh /swapfile
```

##  4. Format as Swap

```sh
sudo mkswap /swapfile
```

## 5. Activate Swap

```sh
sudo swapon /swapfile
```

## 6. Verifying

```sh
sudo swapon --show
free -h
```

## 7. To Make It Permanent (After Reboot)

Edit file ```/etc/fstab```:

```sh
sudo nano /etc/fstab
```

Add this follwing line:

```sh
/swapfile swap swap defaults 0 0
```

## 8. Set Swappiness (Optional)

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

## 9. Delete Swap (Optional)

if it is no longer needed:

```sh
sudo swapoff /swapfile
sudo rm /swapfile
```

Delete following line from ```/etc/fstab```.