# Docker

## Installation

```bash
# debian-based distros
source /etc/os-release
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL "https://download.docker.com/linux/$(. /etc/os-release && echo "$ID")/gpg" \
    | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) \
    signed-by=/etc/apt/keyrings/docker.gpg] \
    https://download.docker.com/linux/$(. /etc/os-release && echo "$ID") \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
    | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```bash
# rhel
export DOCKER_CE=https://mirrors.ustc.edu.cn/docker-ce/
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo ${DOCKER_CE}/linux/rhel/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

## Post-Installation

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

## Connectivity

```bash
# registry
sudo tee /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://docker.1ms.run"
  ]
}
EOF
sudo systemctl daemon-reload && sudo systemctl restart docker
```

```bash
# proxy
sudo tee /etc/docker/daemon.json <<EOF
{
  "proxies": {
    "http-proxy": "http://127.0.0.1:17890",
    "https-proxy": "http://127.0.0.1:17890",
    "no-proxy": "localhost,127.0.0.0/8"
  }
}
EOF
sudo systemctl daemon-reload && sudo systemctl restart docker
```

Docker CE mirrors: [Tuna](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/), [USTC](https://mirrors.ustc.edu.cn/help/docker-ce.html)

Docker Registry: https://status.daocloud.io/status/docker

## Troubleshooting

如果 `docker pull` 没问题但是 `docker compose build --pull` 不行，请尝试 `rm -rf ~/.docker/buildx/`
