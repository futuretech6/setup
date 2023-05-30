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

```powershell
winget install Microsoft.VisualStudioCode
winget install Microsoft.WindowsTerminal
winget install SublimeHQ.SublimeText.4
winget install GyDi.ClashVerge
winget install Git.MinGit

# Administrator
choco install miniconda3
choco install rust
choco install llvm
```

## proxy

```powershell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```
