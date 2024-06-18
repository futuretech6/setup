# Windows

## [Chocolatey](https://github.com/chocolatey/choco)

https://docs.chocolatey.org/en-us/choco/setup#install-with-powershell.exe

```powershell
# Administrator
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

## [Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)

```powershell
# Non-Administrator
irm https://massgrave.dev/get | iex
```

## [winutil](https://github.com/ChrisTitusTech/winutil)

```powershell
# Administrator
iwr -useb https://christitus.com/win | iex
```

## tools

Portable apps will be installed in `%localappdata%\Microsoft\WinGet\Packages`.

```powershell
winget install vscode --id=Microsoft.VisualStudioCode
winget install "Windows Terminal" --id=9N0DX20HK701
winget install --id=SublimeHQ.SublimeText.4
winget install --id=Git.MinGit
winget install --id=GyDi.ClashVerge
winget install quicklook --id=9NV4BS3L1H4S
winget install powertoys --id=XP89DCGQ3K6VLD
winget install JavadMotallebi.NeatDownloadManager
winget install --id=Daum.PotPlayer

# Administrator
choco install miniconda3 --params="/InstallationType=JustMe /AddToPath=1 /RegisterPython=1"  # AddToPath only works for JustMe
choco install rust
choco install llvm
```

## proxy

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```

## WSL

/etc/wsl.conf

```toml
[automount]
enabled = true
options = "uid=1000,gid=1000,umask=22,fmask=11,metadata"

[boot]
command = "service docker start"
```
