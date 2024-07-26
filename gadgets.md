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
# debian (manual)
sudo apt-get install -y python-is-python3
git clone git@github.com:wting/autojump.git ~/.autojump-repo
cd ~/.autojump-repo
./install.py  # remember to add the lines to ~/.zshrc as guided

# fedora
sudo dnf install -y autojump-zsh

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

# pip
pipx install thefuck

# post-installation
echo 'eval $(thefuck --alias fuck)' >> ~/.zshrc
echo "excluded_search_path_prefixes = ['/mnt/']" >> ~/.config/thefuck/settings.py  # WSL only
```

## Proxychains4

```bash
brew install proxychains-ng
sudo apt install proxychains4
sudo dnf install proxychains-ng
```

config:

https://github.com/futuretech6/dotfiles/blob/master/proxychains/proxychains.conf

## [inshellisense](https://github.com/microsoft/inshellisense)

```bash
npm install -g @microsoft/inshellisense
inshellisense bind
```
## [Pandoc](https://github.com/jgm/pandoc)

```bash
sudo apt-get install -y pandoc

brew install pandoc

docker run --rm --volume "`pwd`:/data" --user `id -u`:`id -g` pandoc/latex README.md -o README.pdf
```

## [OneDrive](https://github.com/abraunegg/onedrive)

https://github.com/abraunegg/onedrive#documentation-and-configuration-assistance
- https://github.com/abraunegg/onedrive/blob/master/docs/ubuntu-package-install.md
- https://github.com/abraunegg/onedrive/blob/master/docs/Docker.md

## Marp

https://hub.docker.com/r/marpteam/marp-cli/

```bash
chmod -R 777 $PWD

# server mode
docker run --rm --init -v $PWD:/home/marp/app -e LANG=$LANG -e MARP_USER="$(id -u):$(id -g)" -p 8080:8080 -p 37717:37717 marpteam/marp-cli -s .

# convert to html
docker run --rm -v $PWD:/home/marp/app/ -e LANG=$LANG -e MARP_USER="$(id -u):$(id -g)" marpteam/marp-cli slide-deck.md --allow-local-files
# convert to pdf
docker run --rm --init -v $PWD:/home/marp/app/ -e LANG=$LANG -e MARP_USER="$(id -u):$(id -g)" marpteam/marp-cli slide-deck.md --pdf --allow-local-files
# convert to pptx
docker run --rm --init -v $PWD:/home/marp/app/ -e LANG=$LANG -e MARP_USER="$(id -u):$(id -g)" marpteam/marp-cli slide-deck.md --pptx --allow-local-files
```

## vim-plug

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

cat << EOF >> ~/.vimrc
call plug#begin()

Plug 'prabirshrestha/vim-lsp'
Plug 'mattn/vim-lsp-settings'
Plug 'preservim/nerdtree'

call plug#end()
EOF

:PlugInstall
:LspInstallServer  # in a file
```

## helix

```bash
git clone https://github.com/helix-editor/helix ~/.helix
cargo install --path ~/.helix/helix-term --locked
echo "export HELIX_RUNTIME=$HOME/.helix/runtime" >> ~/.profile
```
