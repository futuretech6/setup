# Linux

## ulimit

```bash
# open files limit
ulimit -n 4096  # tmp
echo "ulimit -n 4096" >> ~/.profile  # persistent
```

## swap

```bash
# add
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# enable
sudo ehco "/swapfile none swap sw 0 0" >> /etc/fstab

# swappiness
sudo sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness
```
