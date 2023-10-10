# macOS (Apple silicon)

## General setup

```bash
# github homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

# ustc homebrew
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/Homebrew/install@HEAD/install.sh)"

echo '# Set PATH, MANPATH, etc., for Homebrew.' >> ~/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
echo '# Set PATH, MANPATH, etc., for Homebrew.' >> ~/.zprofile
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> ~/.zprofile
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> ~/.zprofile
echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"' >> ~/.zprofile

# ustc homebrew cask
brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.ustc.edu.cn/homebrew-cask.git

# oh-my-zsh
brew install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
vim ~/.zshrc # zsh-syntax-highlighting zsh-autosuggestions
```

## Rust

```bash
# disable confirmation prompt
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# [optional]
cargo install gitui bottom bat
```

`vim ~/.cargo/config.toml`

```toml
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index/"

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

[source.rustcc]
registry = "https://code.aliyun.com/rustcc/crates.io-index.git"
```

## LLVM

```bash
LLVM_VERSION=
brew install llvm@$LLVM_VERSION

# [optional]
brew install clang-format@$LLVM_VERSION
```

## Docker

```bash
brew install --cask docker  # VirtualBox is not supported on Apple silicon, so docker-machine can't be used
```

## NodeJs

```bash
brew install npm  # will also install node
# npm install -g n
# sudo n lts
```

## Go

```bash
brew install go

# [optional]
echo "export GOPROXY=https://mirrors.aliyun.com/goproxy/" >> ~/.zprofile
```

## Python

```bash
# install homebrew version (some packages may require newest python version as dep, which will overwrite `/usr/bin/python3`)
brew install python3  # pip3 included

# python binary path
echo 'export PATH="`python3 -m site --user-base`/bin:$PATH"' >> ~/.zprofile

# [optional] pypi mirror
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple  # needs pip >=10.0.0

# conda
mkdir -p ~/.miniconda3
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh -o ~/.miniconda3/miniconda.sh
bash ~/.miniconda3/miniconda.sh -b -u -p ~/.miniconda3
rm -rf ~/.miniconda3/miniconda.sh
~/.miniconda3/bin/conda init zsh

conda config --set auto_activate_base false

# [optional] conda mirror
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

## Solidity

```bash
pip3 install solc-select
solc-select install all
solc-select use $VERSION
```

## Microsoft

```bash
brew install --cask microsoft-edge
brew install --cask visual-studio-code
brew install --cask onedrive
```

## LaTeX

```bash
brew install --cask mactex-no-gui

# for latexindet.pl (https://latexindentpl.readthedocs.io/en/latest/sec-appendices.html#mac)
brew install perl cpanm
cpanm YAML::Tiny File::HomeDir
```

## Redis

```bash

```

## Clash

```bash
brew install --cask clashx
```
