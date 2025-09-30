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
sudo tee /etc/nix/nix.conf <<EOF
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
