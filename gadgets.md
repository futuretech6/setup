# Gadgets

## gitui

For SSH remote

```bash
# echo "eval \`ssh-agent -s\`; ssh-add" >> ~/.profile
eval `ssh-agent -s`                                              
ssh-add
```

## bk

[aeosynth/bk: Terminal Epub reader](https://github.com/aeosynth/bk)

## MPV

[mpv-player/mpv: 🎥 Command line video player](https://github.com/mpv-player/mpv)

## Carbonyl

[fathyb/carbonyl: Chromium running inside your terminal](https://github.com/fathyb/carbonyl)

## autojump

[wting/autojump: A cd command that learns - easily navigate directories from the command line](https://github.com/wting/autojump)

```bash
# manual
sudo apt-get install -y python-is-python3
git clone git@github.com:wting/autojump.git ~/Git/autojump
cd ~/Git/autojump 
./install.py  # remember to add the lines to ~/.zshrc as guided

# macOS
brew install autojump
```

## The Fuck

[nvbn/thefuck: Magnificent app which corrects your previous console command](https://github.com/nvbn/thefuck)

```bash
# Ubuntu
sudo apt install thefuck

# macOS
brew install thefuck

# post-installation
echo 'eval $(thefuck --alias fuck)' >> ~/.zshrc
echo "excluded_search_path_prefixes = ['/mnt/']" >> ~/.config/thefuck/settings.py  # WSL only
```

## Proxychains4

```bash
brew install proxychains-ng
sudo apt install proxychains4
```

config:

```ini
# For wsl2, add the following lines to ~/.profile
#
# hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
# sed -i -E "s/([0-9]{1,3}\.){3}[0-9]{1,3}/$hostip/g" ~/.proxychains/proxychains.conf

dynamic_chain
proxy_dns
quiet_mode

[ProxyList]
socks5 127.0.0.1 7890
http 127.0.0.1 7890
```
