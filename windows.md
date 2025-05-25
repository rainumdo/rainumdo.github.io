# 17

设置 -> 语言 -> 语言选项：(中文) -> 微软拼音 -> 常规 -> 兼容性(开)

# 16

vscodevim
ctrl + shift + p
keybindings.json

```
  {
    "key": "capslock",
    "command": "extension.vim_escape"
  }
```

# 15

add gpedit.msc

```
@echo off

pushd "%~dp0"

dir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~3*.mum >List.txt

dir /b C:\Windows\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~3*.mum >>List.txt

for /f %%i in ('findstr /i . List.txt 2^>nul') do dism /online /norestart /add-package:"C:\Windows\servicing\Packages\%%i"

pause
```

# 14

wsl export

```
wsl -l -v
wsl --shutdown
wsl --export Ubuntu-22.04 D:\Ubuntu-22.04.tar
```

# 13

[Keyboard shortcuts in Windows](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)

# 12

windows services

```
计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services
```

# 11

windows driver manager
[PnPUtil](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/pnputil)

```
pnputil /enum-drivers
pnputil /add-driver x:\driver.inf
pnputil /delete-driver oem0.inf /force
```

# 10

[winows download](https://www.microsoft.com/zh-cn/software-download)

# 9

[windows update catalog](https://www.catalog.update.microsoft.com/home.aspx)

# 8

recovery the BCD(boot configure data)

```
becedit /enum #list the item 

bootrec /scanos
bootrec /fixmbr
bootrec /fixboot
bootrec /rebuildbcd
```

if bootrec /fixboot is denied, do not bootrec /rebuildbcd

```
bootsect /nt60 sys /mbr 
```

and then create the new bcd file:

```
bcdboot c:\windows /s c: 
```

check the bcd:

```
bcdboot c:\windows /v
bcdedit /enum 
```

# 7

edit the right-mouse new content

```
1. run regedit
2. HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Discardable\PostSetup\ShellNew\Classes
3. edit the classes 
```

# 6

reset netwowrk

```
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```

# 5

enable virtual machine for win

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

# 4

install Hyper-V

```
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```

# 3

[update wsl1 to wsl2](https://docs.microsoft.com/zh-cn/windows/wsl/install)

```
wsl -l -v
wsl --set-version
```

# 2

assign ip address

```
ifconfig \release
ifconfig \renew
```

# 1

repair system file

```
DISM /Online /Clean-image /RestoreHealth
```
