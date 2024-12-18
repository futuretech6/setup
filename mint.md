# Linux Mint

## Chinese font

```bash
sudo apt-get install -y fonts-noto-cjk
sudo vim /etc/fonts/conf.avail/64-language-selector-prefer.conf
sudo ln -s /etc/fonts/conf.avail/64-language-selector-prefer.conf /etc/fonts/conf.d/64-language-selector-prefer.conf
```

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Noto Sans CJK SC</family>
      <family>Noto Sans CJK TC</family>
      <family>Noto Sans CJK JP</family>
      <family>Noto Sans CJK KR</family>
    </prefer>
  </alias>
  <alias>
    <family>serif</family>
    <prefer>
      <family>Noto Serif CJK SC</family>
      <family>Noto Serif CJK TC</family>
      <family>Noto Serif CJK JP</family>
      <family>Noto Serif CJK KR</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Noto Sans Mono CJK SC</family>
      <family>Noto Sans Mono CJK TC</family>
      <family>Noto Sans Mono CJK JP</family>
      <family>Noto Sans Mono CJK KR</family>
    </prefer>
  </alias>
</fontconfig>
```

## Chromium

```bash
sudo apt install chromium --target-release=stable  # debian instead of mint
sudo apt-mark hold chromium
sudo apt-mark unhold chromium
```
