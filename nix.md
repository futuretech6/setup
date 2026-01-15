# Nix

## Install

https://nixos.org/download/

```bash
sh <(curl --proto '=https' --tlsv1.2 -L https://nixos.org/nix/install) --daemon
sh <(curl --proto '=https' --tlsv1.2 -L https://mirrors.tuna.tsinghua.edu.cn/nix/latest/install) --daemon
```

## Nix Channels Mirrors

```bash
# 单独安装的 Nix
sudo tee -a /etc/nix/nix.conf <<EOF
substituters = https://mirror.sjtu.edu.cn/nix-channels/store https://cache.nixos.org
EOF

# NixOS
sudo tee /etc/nixos/configuration.nix <<EOF
{ config, lib, pkgs, ... }:
{
  nix.settings.substituters = [ "https://mirror.sjtu.edu.cn/nix-channels/store" ];
}
EOF

# nix-darwin
tee $HOME/.nixpkgs/darwin-configuration.nix <<EOF
{ config, lib, pkgs, ... }:
{
  nix.settings.substituters = [ "https://mirror.sjtu.edu.cn/nix-channels/store" ];
}
EOF
```

```bash
sudo systemctl restart nix-daemon
```

## Features

```bash
# 单独安装的 Nix
sudo tee -a /etc/nix/nix.conf <<EOF
experimental-features = nix-command flakes
EOF
```

## Nixpkgs Mirrors

https://mirrors.tuna.tsinghua.edu.cn/help/nix-channels/

```bash
# 单独安装的 Nix：Nix-Channel
nix-channel --add https://mirrors.tuna.tsinghua.edu.cn/nix-channels/nixpkgs-unstable nixpkgs
nix-channel --update

# 单独安装的 Nix：Flake
nix registry add nixpkgs git+https://mirrors.tuna.tsinghua.edu.cn/git/nixpkgs.git?ref=nixpkgs-unstable

# NixOS
nix-channel --add https://mirrors.tuna.tsinghua.edu.cn/nix-channels/nixos-${VERSION} nixos
nix-channel --update
```

## Use nix pkg as bin

```bash
# 安装
nix profile install ...  # old version
nix profile add nixpkgs#gitui nixpkgs#bottom nixpkgs#bat nixpkgs#erdtree nixpkgs#pre-commit nixpkgs#autojump nixpkgs#dust github:NixOS/nixpkgs/nixos-23.05#nodePackages.prettier
tee -a ~/.$(basename $SHELL)rc <<"EOF"
if [ -f "$HOME/.nix-profile/etc/profile.d/autojump.sh" ]; then
    . "$HOME/.nix-profile/etc/profile.d/autojump.sh"
fi
EOF

# 一次性
tee -a ~/.$(basename $SHELL)rc <<"EOF"
alias gitui="nix run nixpkgs#gitui --"
alias btm="nix run nixpkgs#bottom --"
alias bat="nix run nixpkgs#bat --"
alias erdtree="nix run nixpkgs#erdtree --"
alias pre-commit="nix run nixpkgs#pre-commit --"
alias dust="nix run nixpkgs#dust --"
alias prettier="nix run github:NixOS/nixpkgs/nixos-23.05#nodePackages.prettier --"
export PATH=$(nix build --no-link --print-out-paths nixpkgs#autojump 2>/dev/null)/bin:$PATH
. $(nix build --no-link --print-out-paths nixpkgs#autojump 2>/dev/null)/etc/profile.d/autojump.sh
EOF
```
