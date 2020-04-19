---
title: WSL和windows的zsh终端配置教程
date: 2019-12-23 20:50:16
tags: 
    - shell
    - zsh
    - WSL 
    - windows
    - 终端美化
categories: 技术
---

## WSL终端配置

### 更换配色

背景rgb（0，43，53），文字rgb(147,161,161)

### 下载并安装字体FiraCode，右键属性选择字体为FiraCode

<!-- more -->

### 下载zsh
```shell
sudo apt-get update
sudo apt-get install zsh
```


### 重启wsl安装zsh

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### 配置

```shell
# 自定义主题
cp ~/.oh-my-zsh/themes/agnoster.zsh-theme ~/.oh-my-zsh/custom/themes/agnoster_wsl.zsh-theme

# 找到下面内容，并将blue替换为0.75
# Dir: current working directory
prompt_dir() {
  prompt_segment blue $CURRENT_FG '%~'
}

# 配置主题
vim ~/.zshrc
ZSH_THEME="agnoster_wsl"
# 隐藏用户名+主机名
DEFAULTUSER='cetrol
```

## powershell终端配置

### 管理员运行powershell
```powershell
# 允许powershell运行任何脚本
Set-ExecutionPolicy Bypass
```

### 下载chocolatey安装包管理工具

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

### 安装colortools修改配色

```powershell
choco install colortool
# 查看所有配色
colortool -s
# 设置配色
colortool 配色名字
```

### 安装oh-my-posh

```powershell
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

### 设置主题（熟悉的用户直接到第7步即可）

```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Agnoster
```

### 隐藏用户名主机名

```powershell
$DefaultUser = 'cetrol'
```

### 配置字体

* 下载[powerline字体](https://github.com/powerline/fonts/tree/master/DejaVuSansMono)并安装

* 在powershell中运行下面代码后，右键属性里才可以看到安装的字体

```powershell
Install-Module -Name PSReadLine -Force -SkipPublisherCheck
if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
@"
#requires -Version 2 -Modules posh-git
 
function Write-Theme {
    param(
        [bool]
        `$lastCommandFailed,
        [string]
        `$with
    )
 
    `$lastColor = `$sl.Colors.PromptBackgroundColor
    `$prompt = Write-Prompt -Object `$sl.PromptSymbols.StartSymbol -ForegroundColor `$sl.Colors.PromptForegroundColor -BackgroundColor `$sl.Colors.SessionInfoBackgroundColor
 
    #check the last command state and indicate if failed
    If (`$lastCommandFailed) {
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.FailedCommandSymbol) " -ForegroundColor `$sl.Colors.CommandFailedIconForegroundColor -BackgroundColor `$sl.Colors.SessionInfoBackgroundColor
    }
 
    #check for elevated prompt
    If (Test-Administrator) {
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.ElevatedSymbol) " -ForegroundColor `$sl.Colors.AdminIconForegroundColor -BackgroundColor `$sl.Colors.SessionInfoBackgroundColor
    }
 
    `$user = [System.Environment]::UserName
    `$computer = [System.Environment]::MachineName
    `$path = Get-FullPath -dir `$pwd
    if (Test-NotDefaultUser(`$user)) {
        `$prompt += Write-Prompt -Object "`$user@`$computer " -ForegroundColor `$sl.Colors.SessionInfoForegroundColor -BackgroundColor `$sl.Colors.SessionInfoBackgroundColor
    }
 
    if (Test-VirtualEnv) {
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.SegmentForwardSymbol) " -ForegroundColor `$sl.Colors.SessionInfoBackgroundColor -BackgroundColor `$sl.Colors.VirtualEnvBackgroundColor
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.VirtualEnvSymbol) `$(Get-VirtualEnvName) " -ForegroundColor `$sl.Colors.VirtualEnvForegroundColor -BackgroundColor `$sl.Colors.VirtualEnvBackgroundColor
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.SegmentForwardSymbol) " -ForegroundColor `$sl.Colors.VirtualEnvBackgroundColor -BackgroundColor `$sl.Colors.PromptBackgroundColor
    }
    else {
        `$prompt += Write-Prompt -Object "`$(`$sl.PromptSymbols.SegmentForwardSymbol) " -ForegroundColor `$sl.Colors.SessionInfoBackgroundColor -BackgroundColor `$sl.Colors.PromptBackgroundColor
    }
 
    ## Writes the drive portion
    `$prompt += Write-Prompt -Object "`$path " -ForegroundColor `$sl.Colors.PromptForegroundColor -BackgroundColor `$sl.Colors.PromptBackgroundColor
 
    `$status = Get-VCSStatus
    if (`$status) {
        `$themeInfo = Get-VcsInfo -status (`$status)
        `$lastColor = `$themeInfo.BackgroundColor
        `$prompt += Write-Prompt -Object `$(`$sl.PromptSymbols.SegmentForwardSymbol) -ForegroundColor `$sl.Colors.PromptBackgroundColor -BackgroundColor `$lastColor
        `$prompt += Write-Prompt -Object " `$(`$themeInfo.VcInfo) " -BackgroundColor `$lastColor -ForegroundColor `$sl.Colors.GitForegroundColor
    }
 
    ## Writes the postfix to the prompt
    `$prompt += Write-Prompt -Object `$sl.PromptSymbols.SegmentForwardSymbol -ForegroundColor `$lastColor
 
    `$timeStamp = Get-Date -UFormat %r
    `$timestamp = "[`$timeStamp]"
 
    `$prompt += Set-CursorForRightBlockWrite -textLength (`$timestamp.Length + 1)
    `$prompt += Write-Prompt `$timeStamp -ForegroundColor `$sl.Colors.PromptForegroundColor
 
    `$prompt += Set-Newline
 
    if (`$with) {
        `$prompt += Write-Prompt -Object "`$(`$with.ToUpper()) " -BackgroundColor `$sl.Colors.WithBackgroundColor -ForegroundColor `$sl.Colors.WithForegroundColor
    }
    `$prompt += Write-Prompt -Object (`$sl.PromptSymbols.PromptIndicator) -ForegroundColor `$sl.Colors.PromptBackgroundColor
    `$prompt += ' '
    `$prompt
}
 
`$sl = `$global:ThemeSettings #local settings
`$sl.PromptSymbols.StartSymbol = ''
`$sl.PromptSymbols.PromptIndicator = [char]::ConvertFromUtf32(0x276F)
`$sl.PromptSymbols.SegmentForwardSymbol = [char]::ConvertFromUtf32(0xE0B0)
`$sl.Colors.PromptForegroundColor = [ConsoleColor]::White
`$sl.Colors.PromptSymbolColor = [ConsoleColor]::White
`$sl.Colors.PromptHighlightColor = [ConsoleColor]::DarkBlue
`$sl.Colors.GitForegroundColor = [ConsoleColor]::Black
`$sl.Colors.WithForegroundColor = [ConsoleColor]::DarkRed
`$sl.Colors.WithBackgroundColor = [ConsoleColor]::Magenta
`$sl.Colors.VirtualEnvBackgroundColor = [System.ConsoleColor]::Red
`$sl.Colors.VirtualEnvForegroundColor = [System.ConsoleColor]::White
"@>$env:userprofile"\Documents\WindowsPowerShell\Modules\oh-my-posh\2.0.263\Themes\Paradox.psm1"       # 建议自己把代码放到记事本然后替换路径，此条路径要改。。
@"
chcp 65001
Set-PSReadlineOption -EditMode Emacs
function which($name) { Get-Command $name | Select-Object Definition }
function rmrf($item) { Remove-Item $item -Recurse -Force }
function mkfile($file) { "" | Out-File $file -Encoding ASCII }
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Agnoster
$DefaultUser = 'cetrol'

## Chocolatey profile
$ChocolateyProfile = "$env:ChocolateyInstall\helpers\chocolateyProfile.psm1"
if (Test-Path($ChocolateyProfile)) {
  Import-Module "$ChocolateyProfile"
}
```


## Vscode终端配置

* 设置->用户->功能->终端  设置可以正确显示Agnoster的字体，比如fira code retina字体

