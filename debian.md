# Debian

## General setup

```bash
sudo apt-get update
sudo apt-get install -y wget curl git vim

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

## Rust

```bash
# RUSTUP_UNPACK_RAM=200000000
# RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
# RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
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
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# ubuntu
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# post installation
sudo groupadd docker
sudo usermod -aG docker $USER
```

[Tuna](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)

[USTC](https://mirrors.ustc.edu.cn/help/docker-ce.html)

## NodeJs

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
source ~/.zshrc
nvm install --lts
nvm use --lts

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

# conda config
~/.miniconda3/bin/conda init zsh
conda config --set auto_activate_base false

# [optional] conda mirror
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

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

## Brave

https://brave.com/linux/#debian-ubuntu-mint

```bash
# set proxy for apt
echo 'Acquire::http::Proxy "http://username:password@127.0.0.1:7890/";' | sudo tee -a /etc/apt/apt.conf
echo 'Acquire::https::Proxy "http://username:password@127.0.0.1:7890/";' | sudo tee -a /etc/apt/apt.conf 
```

```bash
sudo curl -fsSLo /etc/apt/keyrings/brave-browser-archive-keyring.gpg \
  https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main" \
  | sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt-get update
sudo apt-get install -y brave-browser
```
