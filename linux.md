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

<details>
  <summary><h2>nginx certificates (deprecated)</h2></summary>

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

</details>

## acme.sh

```bash
curl https://get.acme.sh | sh -s email=xxx

# 按顺序分别测试 cf、go、ali、tx 的 doh，如果前两者都无法连接，则会 fallback 到 alidns，
# 但是 alidns 对 TXT 字段修改的响应很慢，**可能**导致失败。
sed -i "s/dns.alidns.com/whatever.doh.provider/" ~/.acme.sh/acme.sh

export SITE=my-domain.duckdns.org

# issue and install
export DuckDNS_Token=""
acme.sh --issue --dns dns_duckdns -d ${SITE} --server letsencrypt --reloadcmd "sudo systemctl reload nginx" --debug --log
sudo mkdir -p -m=777 /etc/nginx/ssl/ && acme.sh --install-cert -d ${SITE} \
    --key-file       /etc/nginx/ssl/${SITE}.key  \
    --fullchain-file /etc/nginx/ssl/${SITE}.fullchain.cer \
    --reloadcmd      "sudo systemctl reload nginx"

# force renew
export DuckDNS_Token=""
acme.sh --renew -d ${SITE} --server letsencrypt --force
```

nginx config:

```conf
ssl_certificate     /etc/nginx/ssl/${SITE}.fullchain.cer;
ssl_certificate_key /etc/nginx/ssl/${SITE}.key;
```

## vaultwarden

[official wiki](https://github.com/dani-garcia/vaultwarden/wiki/Proxy-examples)'s proxy_pass settings in "Nginx with sub-path" are incorrect.

/etc/nginx/sites-available/default

```diff
location /vaultwarden/ {
    ...
-   proxy_pass http://vaultwarden-default;
+   proxy_pass http://vaultwarden-default/vaultwarden/;
}
```

compose.yaml

```yaml
environment: [DOMAIN=https://my.domain.com/vaultwarden]
```

Check https://my.domain.com/vaultwarden/api/config to validate.

## Cloudflare DNS

https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/do-more-with-tunnels/local-management/as-a-service/linux/

```bash
sudo mkdir -p --mode=0755 /usr/share/keyrings
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main' \
    | sudo tee /etc/apt/sources.list.d/cloudflared.list
sudo apt-get update && sudo apt-get install cloudflared
```

## CUDA

```bash
# WSL2
echo "export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH" >> ~/.profile
```

## WSL NFS Mount

https://github.com/microsoft/WSL/issues/12508#issuecomment-2764782989

```bash
sudo mount -t nfs -o noresvport -a --verbose

# 写到 /etc/fstab 里
192.168.1.100:/data /mnt/nfs nfs defaults,noresvport,proto=tcp,nolock,_netdev,x-systemd.automount 0 0
```

- `noresvport`：强制使用非保留端口（1024 以上），解决 WSL 权限不足无法使用低位端口的问题。
- `proto=tcp`：在 WSL 环境下，TCP 比 UDP 更稳定。
- `nolock`：禁用文件锁协议（NLM）。WSL2 的网络架构有时无法很好地处理 NFS 锁，不加这个参数可能会导致挂载时卡住或应用报错。
- `defaults`：包含一组默认选项（如 `rw`, `suid`, `dev`, `exec`, `auto`, `nouser`, `async`）。
- `_netdev`：告诉系统这是一个网络设备，等待网络就绪后再挂载。
- `x-systemd.automount`：当你第一次访问该目录时才真正触发挂载，这在 WSL 环境下非常有效，能避免因启动顺序导致的挂载超时。
