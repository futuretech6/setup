# Debian

## General setup

```bash
sudo apt-get update
sudo apt-get install -y wget curl git vim
git config --global core.editor "vim"

# oh-my-zsh
sudo apt-get install -y zsh
# sh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ohmyzsh/ohmyzsh@master/tools/install.sh)"  # cdn -> fastly
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
sed -i "s/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/g" ~/.zshrc
sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="ys"/' ~/.zshrc
echo "source ~/.profile" >> ~/.zshrc
echo "zstyle ':omz:update' mode auto" >> ~/.zshrc

# locale for zh_CN
locale-gen zh_CN.UTF-8
sudo update-locale LANG=zh_CN.UTF-8 LANGUAGE=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8
```

## Flatpak

```bash
sudo apt install flatpak
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo  # require restart
# sudo flatpak remote-add --if-not-exists flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
sudo flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub

# optional
sudo apt install gnome-software-plugin-flatpak
sudo apt install plasma-discover-backend-flatpak

# font
ln -s /etc/fonts/ ~/.var/app/com.qq.QQmusic/config/fontconfig
```

## Rust

```bash
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static

export RUSTUP_UPDATE_ROOT=https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup
export RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup
```

```bash
# export RUSTUP_UNPACK_RAM=200000000
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y  # disable confirmation prompt

# [optional]
cargo install gitui bottom bat erdtree mdcat  # needs cc, make, pkg-config, libssl-dev
```

`vim ~/.cargo/config.toml`

https://github.com/futuretech6/dotfiles/blob/master/rust/config.toml

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
echo '{ "registry-mirrors": [ "https://docker.kubesre.xyz" ] }' | sudo tee /etc/docker/daemon.json

# proxy: sudo vim /lib/systemd/system/docker.service
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1"
```

Docker CE mirrors: [Tuna](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/), [USTC](https://mirrors.ustc.edu.cn/help/docker-ce.html)

Docker Registry: [Docker Proxy](https://dockerproxy.com/docs), ~~[kubesre/docker-registry-mirrors](https://github.com/kubesre/docker-registry-mirrors)~~, [dqzboy/Docker-Proxy](https://github.com/dqzboy/Docker-Proxy)

## NodeJs

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
source ~/.zshrc
nvm install --lts
nvm use --lts

npm config set registry https://registry.npmmirror.com

# yarn (https://yarnpkg.com/getting-started/install)
corepack enable    # Node.js >=16.10
npm i -g corepack  # Node.js <16.10
corepack prepare yarn@stable --activate  # Node.js ^16.17 or >=18.6
## [optional]
yarn set version 1.22.19
```

## Go

```bash
# from pkg
sudo apt-get install -y golang
# from binary (https://go.dev/dl/)
wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin

# [optional]
echo "export GOPROXY=https://mirrors.aliyun.com/goproxy/" >> ~/.profile
```

## Python

```bash
# [optional] pypi mirror
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple  # needs pip >=10.0.0

# conda mirror https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/?C=M&O=D
wget ...
bash ...

# miniconda https://docs.conda.io/projects/miniconda/en/latest/
mkdir -p ~/.miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/.miniconda3/miniconda.sh
bash ~/.miniconda3/miniconda.sh -b -u -p ~/.miniconda3
rm -rf ~/.miniconda3/miniconda.sh

# miniforge
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
rm -f Miniforge3-$(uname)-$(uname -m).sh

# conda config
~/.miniconda3/bin/conda init zsh
conda config --set auto_activate_base false

# [optional] conda mirror
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

# mamba
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)

# [optional] debian global pip
pip config set global.break-system-packages true

# https://github.com/pypa/pipx
python3 -m pip install --user pipx
python3 -m pipx ensurepath
```

## Solidity

```bash
pipx install solc-select
solc-select install all
solc-select use $VERSION
```

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

## Clash

```bash
# from github release
sudo apt-get install -y wget gzip
wget https://github.com/Dreamacro/clash/releases/download/v1.10.0/clash-linux-amd64-v1.10.0.gz
gunzip clash-linux-amd64-v1.10.0.gz
mkdir -p ~/App/clash
mv clash-linux-amd64-v1.10.0 ~/App/clash/clash
chmod +x ~/App/clash/clash
# from gopkg
go install github.com/Dreamacro/clash@latest

wget https://github.com/Dreamacro/maxmind-geoip/releases/latest/download/Country.mmdb -O ~/App/clash/Country.mmdb
# wget https://cdn.jsdelivr.net/gh/Dreamacro/maxmind-geoip@release/Country.mmdb -O ~/App/clash/Country.mmdb
~/App/clash/clash -f ~/App/clash/config.yaml -d ~/App/clash

# [optional]
sudo vim /lib/systemd/system/clash@.service
sudo systemctl enable clash@`whoami`
```

`clash@.service`

```
[Unit]
Description=A rule based proxy in Go for %i.
After=network.target

[Service]
Type=simple
User=%i
Restart=on-abort
ExecStart=/home/%i/App/clash/clash -f /home/%i/App/clash/config.yaml -d /home/%i/App/clash

[Install]
WantedBy=multi-user.target
```

## CUDA

```bash
# WSL2
echo "export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH" >> ~/.profile
```

## Browser

```bash
# set proxy for apt
echo 'Acquire::http::Proxy "http://username:password@127.0.0.1:7890/";' | sudo tee -a /etc/apt/apt.conf
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

## Rime

[RIME | 中州韻輸入法引擎](https://rime.im/), [iDvel/rime-ice: Rime 配置：雾凇拼音 | 长期维护的简体词库](https://github.com/iDvel/rime-ice), [東風破 /plum/: Rime configuration manager](https://github.com/rime/plum)

```bash
# fcitx
sudo apt-get install -y fcitx-rime
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx-rime bash -s -- "iDvel/rime-ice:others/recipes/full"

im-config -n fcitx

mkdir -p ~/.config/environment.d/
echo 'export INPUT_METHOD=fcitx' > ~/.config/environment.d/im.conf
echo 'export GTK_IM_MODULE=fcitx' >> ~/.config/environment.d/im.conf
echo 'export QT_IM_MODULE=fcitx' >> ~/.config/environment.d/im.conf
echo 'export XMODIFIERS=@im=fcitx' >> ~/.config/environment.d/im.conf

mkdir -p ~/.config/autostart && cp /usr/share/applications/fcitx.desktop ~/.config/autostart

# fcitx5
sudo apt-get install -y fcitx5 fcitx5-rime
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx5-rime bash -s -- "iDvel/rime-ice:others/recipes/full"

im-config -n fcitx5

mkdir -p ~/.config/environment.d/
echo 'export INPUT_METHOD=fcitx5' > ~/.config/environment.d/im.conf
echo 'export GTK_IM_MODULE=fcitx5' >> ~/.config/environment.d/im.conf
echo 'export QT_IM_MODULE=fcitx5' >> ~/.config/environment.d/im.conf
echo 'export XMODIFIERS=@im=fcitx5' >> ~/.config/environment.d/im.conf

echo 'export INPUT_METHOD=fcitx5' >> ~/.profile  # chrome use .profile rather than environment.d
echo 'export GTK_IM_MODULE=fcitx5' >> ~/.profile
echo 'export QT_IM_MODULE=fcitx5' >> ~/.profile
echo 'export XMODIFIERS=@im=fcitx5' >> ~/.profile

mkdir -p ~/.config/autostart && cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart
```
如果第三方应用还是没法用输入法，就尝试切换把 DE 从 Wayland 切换为 Xorg

