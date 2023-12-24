# Fedora

## General

```bash

# oh-my-zsh
dnf install zsh
# sh -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ohmyzsh/ohmyzsh@master/tools/install.sh)"  # cdn -> fastly
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
sed -i "s/plugins=(git)/plugins=(git zsh-syntax-highlighting zsh-autosuggestions)/g" ~/.zshrc
sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="ys"/' ~/.zshrc
echo "source ~/.profile" >> ~/.zshrc
echo "zstyle ':omz:update' mode auto" >> ~/.zshrc
```

## Software sources

```bash
sudo sed -e 's|^metalink=|#metalink=|g' \
         -e 's|^#baseurl=http://download.example/pub/fedora/linux|baseurl=https://mirrors.ustc.edu.cn/fedora|g' \
         -i.bak \
         /etc/yum.repos.d/fedora.repo \
         /etc/yum.repos.d/fedora-modular.repo \
         /etc/yum.repos.d/fedora-updates.repo \
         /etc/yum.repos.d/fedora-updates-modular.repo
```

## Desktop

[Switching desktop environments](https://docs.fedoraproject.org/en-US/quick-docs/switching-desktop-environments)

## Docker

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl start docker

# post installation
sudo groupadd docker
sudo usermod -aG docker $USER
```

## Microsoft

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'

dnf check-update
sudo dnf install code # or code-insiders
```

## Brave

```bash
sudo dnf install dnf-plugins-core
sudo dnf config-manager --add-repo https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo
sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc
sudo dnf install brave-browser
```