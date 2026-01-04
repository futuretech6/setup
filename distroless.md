# Distroless

Distro-irrelevant setup scripts for Linux.

## Generic

```bash
# omz
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# sh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ohmyzsh/ohmyzsh@master/tools/install.sh)"  # cdn -> fastly
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
sed -i "s/plugins=(git)/plugins=(git docker docker-compose zsh-syntax-highlighting zsh-autosuggestions)/g" ~/.zshrc
sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="ys"/' ~/.zshrc
echo "source ~/.profile" >> ~/.zshrc
echo "zstyle ':omz:update' mode auto" >> ~/.zshrc

# locale for zh_CN
locale-gen zh_CN.UTF-8
sudo update-locale LANG=zh_CN.UTF-8 LANGUAGE=zh_CN.UTF-8 LC_ALL=zh_CN.UTF-8

# 启用 xxxrc 里的 alias
sed -i "s/#[ \t]*alias /alias /g" "$HOME/.$(basename $SHELL)rc"

# 实时更新 ~/.bash_history（仅用于 bash，zsh 等语法不同）
cat <<"EOF" >> ~/.bashrc
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
shopt -s histappend
EOF
```



## bash-completion

```bash
# Config
# sudo tee -a /etc/inputrc <<EOF
cat << EOF > ~/.inputrc
\$include /etc/inputrc  # 若是添加到 /etc/inputrc 则删除这行
set show-all-if-ambiguous On
set completion-ignore-case On
set colored-stats Off
set colored-completion-prefix On
set mark-directories On
"\e[A": history-search-backward
"\e[B": history-search-forward
EOF

bind -f ~/.inputrc

# Plugins
curl https://raw.githubusercontent.com/docker/cli/master/contrib/completion/bash/docker | sudo tee /etc/bash_completion.d/docker
mkdir -p ~/.bash_completion.d && curl https://raw.githubusercontent.com/docker/cli/master/contrib/completion/bash/docker > ~/.bash_completion.d/docker.sh
# cat <<"EOF" >> ~/.bashrc
# if [ -f ~/.bash_completion.d/docker.sh ]; then
#     source ~/.bash_completion.d/docker.sh
# fi
# EOF
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
# from binary (https://go.dev/dl/)
sudo rm -rf /usr/local/go && \
proxychains curl -L "https://go.dev/dl/$(proxychains curl -s "https://go.dev/dl/?mode=json" \
                                         | jq -r '.[0].version').linux-$(dpkg --print-architecture).tar.gz" \
    | sudo tar -C /usr/local -xz
echo "export PATH=/usr/local/go/bin:\$PATH" >> ~/.profile

# [optional]
echo "export GOPROXY=https://mirrors.aliyun.com/goproxy/" >> ~/.profile
go env -w GOPROXY=https://goproxy.io,direct
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

# or global
sudo pip config set global.break-system-packages true
sudo pip install pipx
sudo pipx install --global pre-commit

# uv
curl -LsSf https://astral.sh/uv/install.sh | sh
sudo mkdir -p /etc/uv && sudo tee /etc/uv/uv.toml <<EOF
python-install-mirror = "https://gh-proxy.com/github.com/astral-sh/python-build-standalone/releases/download"
[[index]]
url = "https://mirrors.ustc.edu.cn/pypi/simple"
default = true
EOF
```
