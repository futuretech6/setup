# Rime

[RIME | 中州韻輸入法引擎](https://rime.im/), [iDvel/rime-ice: Rime 配置：雾凇拼音 | 长期维护的简体词库](https://github.com/iDvel/rime-ice), [東風破 /plum/: Rime configuration manager](https://github.com/rime/plum)

<details>
  <summary><h2>fcitx4</h2></summary>

```bash
sudo apt-get install -y fcitx-rime

curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx-rime bash -s -- :all
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx-rime bash -s -- iDvel/rime-ice:others/recipes/full

im-config -n fcitx

mkdir -p ~/.config/autostart && cp /usr/share/applications/fcitx.desktop ~/.config/autostart

mkdir -p ~/.config/environment.d/ && cat <<EOF > ~/.config/environment.d/im.conf
INPUT_METHOD=fcitx
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=fcitx
EOF

cat <<EOF >> ~/.profile
export INPUT_METHOD=fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
export GLFW_IM_MODULE=fcitx
EOF
```

</details>

## fcitx5

```bash
sudo apt-get install -y fcitx5 fcitx5-rime fcitx5-material-color
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx5-rime bash -s -- :all
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx5-rime bash -s -- iDvel/rime-ice:others/recipes/full

im-config -n fcitx5

mkdir -p ~/.config/autostart && cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart

mkdir -p ~/.config/environment.d/ && cat <<EOF > ~/.config/environment.d/im.conf
INPUT_METHOD=fcitx
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=fcitx
EOF

cat <<EOF >> ~/.profile
export INPUT_METHOD=fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
export GLFW_IM_MODULE=fcitx
EOF
```

## fcitx5 (flatpak)

若 librime 版本过低使用

```bash
sudo apt-get install -y ibus  # need a front-end in host, using fcitx4(5) as front-end is horrible in Ubuntu 22.04
flatpak install -y flathub org.fcitx.Fcitx5 && flatpak install -y flathub org.fcitx.Fcitx5.Addon.Rime
curl -sSL https://raw.githubusercontent.com/rime/plum/master/rime-install \
  | plum_dir=$HOME/.plum rime_frontend=fcitx5-rime rime_dir=$HOME/.var/app/org.fcitx.Fcitx5/data/fcitx5/rime/ \
    bash -s -- "iDvel/rime-ice:others/recipes/full"

im-config -n ibus

mkdir -p ~/.config/autostart && cp /var/lib/flatpak/exports/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart/fcitx5-flatpak.desktop

mkdir -p ~/.config/environment.d/ && cat <<EOF > ~/.config/environment.d/im.conf
INPUT_METHOD=ibus
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
SDL_IM_MODULE=ibus
GLFW_IM_MODULE=ibus
EOF

cat <<EOF >> ~/.profile
export INPUT_METHOD=ibus
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export SDL_IM_MODULE=ibus
export GLFW_IM_MODULE=ibus
EOF
```

## user config

https://github.com/rime/home/wiki/CustomizationGuide#定製指南

```bash
export RIME_CONFIG_DIR=~/.config/fcitx/rime  # fcitx4
export RIME_CONFIG_DIR=~/.local/share/fcitx5/rime  # fcitx5
export RIME_CONFIG_DIR=~/.var/app/org.fcitx.Fcitx5/data/fcitx5/rime  # fcitx5 (flatpak)

# sed -i -E 's/page_size:\s*[0-9]+/page_size: 8/' $RIME_CONFIG_DIR/default.yaml
# sed -i 's/^\(\s*\)\(-\s*Control+grave\)/\1# \2/' $RIME_CONFIG_DIR/default.yaml

cat > $RIME_CONFIG_DIR/default.custom.yaml <<EOF
patch:
  config_version: '$(date +"%Y-%m-%d")'
  schema_list: [schema: rime_ice]
  menu/page_size: 8
  switcher/hotkeys: [F4]
EOF

cat > $RIME_CONFIG_DIR/rime_ice.custom.yaml <<EOF
patch:
  speller/algebra/+:
    - derive/ang$/an/
    - derive/an$/ang/
    - derive/eng$/en/
    - derive/en$/eng/
    - derive/in$/ing/
    - derive/ing$/in/
    - derive/ian$/iang/
EOF
```

## Applictions

在 `/usr/share/applications/` 中，修改 `Exec` 字段，如 `bash -c "source ~/.profile && /usr/bin/wechat %U"`

## Diagnose

如果第三方应用还是没法在 Wayland 下使用输入法，两种解决方案：

1. 尝试把 DE 切换为 Xorg 版本的
2. 如果是 Gnome，可以安装插件 [Input Method Panel](https://extensions.gnome.org/extension/261/kimpanel/)

仍有问题：`fcitx5-diagnose` 或 `flatpak run --command=fcitx5-diagnose org.fcitx.Fcitx5`
