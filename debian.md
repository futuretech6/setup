# Debian

## General setup

```bash
sudo apt-get update
sudo apt-get install -y wget curl git vim
git config --global core.editor "vim"
```

## Flatpak

```bash
sudo apt install flatpak
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo  # require restart
# sudo flatpak remote-add --if-not-exists flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
sudo flatpak remote-modify flathub --url=https://mirrors.ustc.edu.cn/flathub
sudo flatpak remote-modify flathub --url=https://dl.flathub.org/repo  # reset

# optional
sudo apt install gnome-software-plugin-flatpak
sudo apt install plasma-discover-backend-flatpak

# font
ln -s /etc/fonts/ ~/.var/app/com.qq.QQmusic/config/fontconfig
```

## LLVM

```bash
LLVM_VERSION=
sudo bash -c "$(wget -O - https://apt.llvm.org/llvm.sh)" $LLVM_VERSION
echo 'export CC=`which clang-$LLVM_VERSION` ; export CXX=`which clang++-$LLVM_VERSION`' >> ~/.profile

# [optional]
sudo apt-get install -y clangd-$LLVM_VERSION clang-format-$LLVM_VERSION clang-tidy-$LLVM_VERSION
```

## Docker

```bash
# debian
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# ubuntu
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# post installation
sudo groupadd docker
sudo usermod -aG docker $USER

# registry
echo '{ "registry-mirrors": ["https://docker.m.daocloud.io"] }' | sudo tee /etc/docker/daemon.json
sudo systemctl daemon-reload && sudo systemctl restart docker

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

## Microsoft

```bash
sudo apt-get install -y wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor -o /etc/apt/keyrings/packages.microsoft.gpg

# edge
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/edge stable main" > /etc/apt/sources.list.d/microsoft-edge.list'
sudo apt-get update
sudo apt-get install -y microsoft-edge-stable

# vscode
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt-get update
sudo apt-get install -y code
```

## Redis

```bash
sudo apt-get install -y lsb-release
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
sudo apt-get update
sudo apt-get install -y redis
sudo service redis-server start  # wsl
```

## Browser

```bash
# set proxy for apt
echo 'Acquire::http::Proxy "http://username:password@127.0.0.1:7890/";' | sudo tee /etc/apt/apt.conf
echo 'Acquire::https::Proxy "http://username:password@127.0.0.1:7890/";' | sudo tee -a /etc/apt/apt.conf
```

**Chrome**

https://www.google.com/linuxrepositories

```bash
proxychains wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/google.gpg >/dev/null
echo "deb [arch=$(dpkg --print-architecture)] https://dl.google.com/linux/chrome/deb/ stable main" \
  | sudo tee /etc/apt/sources.list.d/google-chrome.list  # default filename

sudo apt-get update
sudo apt-get install -y google-chrome-stable
```

**Firefox**

https://support.mozilla.org/en-US/kb/install-firefox-linux

```bash
wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" \
  | sudo tee /etc/apt/sources.list.d/mozilla.list > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://mirrors.ustc.edu.cn/mozilla/apt mozilla main" \
  | sudo tee /etc/apt/sources.list.d/mozilla.list > /dev/null  # 镜像

sudo apt-get update
sudo apt-get install -y firefox/mozilla  # not the snap version
sudo apt-mark hold firefox               # fuck canonical and snap

sudo apt upgrade firefox/mozilla && sudo apt-mark hold firefox/mozilla  # for upgradation
```

**Brave**

https://brave.com/linux/#debian-ubuntu-mint

```bash
proxychains wget -q -O - https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg | sudo tee /etc/apt/keyrings/brave.gpg >/dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main" \
  | sudo tee /etc/apt/sources.list.d/brave-browser.list

sudo apt-get update
sudo apt-get install -y brave-browser
```

**Vivaldi**

```bash
proxychains wget -q -O - https://repo.vivaldi.com/stable/linux_signing_key.pub | gpg --dearmor | sudo tee /etc/apt/keyrings/vivaldi.gpg >/dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/vivaldi.gpg] https://repo.vivaldi.com/stable/deb stable main" \
  | sudo tee /etc/apt/sources.list.d/vivaldi-browser.list

sudo apt-get update
sudo apt-get install -y vivaldi-stable
```

## Qemu

https://wiki.debian.org/QEMU

```bash
sudo apt install qemu-system
```

## Deb Multimedia

'deb-multimedia.org', not the 'Debian Multimedia Maintainers'. [^deb-multimedia]

[^deb-multimedia]: https://wiki.debian.org/DebianMultimedia/FAQ

```bash
curl -L https://mirrors.ustc.edu.cn/deb-multimedia/pool/main/d/deb-multimedia-keyring/deb-multimedia-keyring_2024.9.1_all.deb -o /tmp/deb-multimedia-keyring.deb
sudo apt-get install -y /tmp/deb-multimedia-keyring.deb
rm /tmp/deb-multimedia-keyring.deb

echo "deb https://mirrors.ustc.edu.cn/deb-multimedia/ $(lsb_release -cs) main non-free" \
  | sudo tee /etc/apt/sources.list.d/deb-multimedia.list
echo "deb https://mirrors.ustc.edu.cn/deb-multimedia/ $(lsb_release -cs)-backports main" \
  | sudo tee /etc/apt/sources.list.d/deb-multimedia.list
```

## Debian CN

https://github.com/debiancn/repo/blob/master/README.rst

```bash
echo "deb https://repo.debiancn.org/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/debiancn.list;
wget https://repo.debiancn.org/pool/main/d/debiancn-keyring/debiancn-keyring_0~20161212_all.deb -O /tmp/debiancn-keyring.deb;
sudo apt install /tmp/debiancn-keyring.deb;
sudo apt update;
rm /tmp/debiancn-keyring.deb;
```

# Spotify

```bash
curl -sS https://download.spotify.com/debian/pubkey_C85668DF69375001.gpg | sudo gpg --dearmor --yes -o /etc/apt/trusted.gpg.d/spotify.gpg
echo "deb https://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list
sudo apt-get update && sudo apt-get install -y spotify-client
```
