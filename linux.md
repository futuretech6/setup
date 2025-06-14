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
echo "/swapfile none swap sw 0 0" | sudo tee -a /etc/fstab

# swappiness
sudo sysctl vm.swappiness=10  # temporary
sudo sed -i.bak 's/vm\.swappiness\s*=\s*[0-9]*/vm.swappiness = 10/' /etc/sysctl.conf  # permanent
cat /proc/sys/vm/swappiness
```

## nginx certificates

```bash
# ZeroSSL verify (using apache)
scp xxx.txt server:/var/www/html/.well-known/pki-validation/

# ZeroSSL certificates installation
unzip example.com.zip -d certificates
cat certificates/certificate.crt certificates/ca_bundle.crt | sudo tee /etc/ssl/certificate.crt.merge > /dev/null
sudo cp certificates/private.key /etc/ssl/private/private.key
rm -rf certificates/
sudo systemctl restart nginx.service
```

## acme.sh

```bash
export DuckDNS_Token=""
acme.sh --issue --dns dns_duckdns -d my-domain.duckdns.org --server letsencrypt --debug --log

# force renew
export DuckDNS_Token=""
acme.sh --renew -d my-domain.duckdns.org --server letsencrypt --force
```
