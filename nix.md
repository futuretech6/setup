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
# 单独安装的 Nix
nix-channel --add https://mirrors.tuna.tsinghua.edu.cn/nix-channels/nixpkgs-unstable nixpkgs
nix-channel --update

# NixOS
nix-channel --add https://mirrors.tuna.tsinghua.edu.cn/nix-channels/nixos-${VERSION} nixos
nix-channel --update
```

## Use nix pkg as bin

```bash
alias gitui="nix run nixpkgs#gitui --"
alias btm="nix run nixpkgs#bottom --"
alias bat="nix run nixpkgs#bat --"
alias erdtree="nix run nixpkgs#erdtree --"
```
