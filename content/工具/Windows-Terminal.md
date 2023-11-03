---
title: Windows Terminal 增强
tags:
  - 开发工具
  - windows
  - terminal
---
## 💻 Windows Terminal

🏠 https://github.com/microsoft/terminal

## 📦 scoop

Scoop 是 Windows 的命令行安装程序。

⬇ https://github.com/ScoopInstaller/Install

```powershell
# 获取安装文件
irm get.scoop.sh -outfile 'install.ps1'

# 执行安装（安装文件里有参数说明）
# ScoopDir 是 scoop 的安装目录
# ScoopGlobalDir 是 scoop 全局 app 的安装目录
# Proxy 安装时的代理设置
.\install.ps1 -ScoopDir 'D:\Applications\Scoop' -ScoopGlobalDir 'F:\GlobalScoopApps'
```

```powershell
# 使用
scoop install coreutils
```

## 🎨 美化

### 📝 文件图标

```powershell
Install-Module -Name Terminal-Icons -Repository PSGallery
```

use scoop:
```powershell
scoop bucket add extras
scoop install terminal-icons
```

### 😮 oh-my-posh (建议使用 starship )

```powershell
Set-ExecutionPolicy Bypass -Scope LocalMachine -Force; Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://ohmyposh.dev/install.ps1'))
```

### 🖋 设置字体

https://ohmyposh.dev/docs/installation/fonts

```powershell
oh-my-posh font install
```

### ✨[Starship](https://starship.rs/)

依赖 [Nerd Font](https://www.nerdfonts.com/) 字体。

安装：
```powershell
# use cargo 
cargo install starship --locked
# use scoop
scoop install starship
# use winget
winget install --id Starship.Starship
```

配置 `powershell` ，打开 `$PROFILE` 配置文件，添加如下内容：

```powershell
Invoke-Expression (&starship init powershell)
```

默认配置文件路径 `~/.config/starship.toml` ，或者设置环境变量 `STARSHIP_CONFIG` 指定配置文件路径。

可以根据官方的[文档](https://starship.rs/config/)自己配置主题，或者使用提供的几个[配置](https://starship.rs/presets/#nerd-font-symbols)

## 🚀 补全

**相关项目：**

* https://github.com/abgox/PS-completions
* https://github.com/PowerShell-Completion

```powershell
Install-Module PSReadLine -Scope AllUsers
Install-Module posh-git -Scope AllUsers
Install-Module yarn-completion -Scope AllUsers
Install-Module npm-completion -Scope AllUsers
```

### 🛠 配置

```powershell
notepad $PROFILE
```

```powershell
$PSDefaultParameterValues['*:Language'] = 'zh-CN'

Import-Module 'E:\development\vcpkg\scripts\posh-vcpkg'

# 自定义补全脚本
$completionsPath = "D:\Users\user\Documents\PowerShell\completions"
# Get all completion script files in the specified directory
$completionScripts = Get-ChildItem -Path $completionsPath -Filter "*.ps1" -File
# Load each completion script
foreach ($script in $completionScripts) {
    . $script.FullName
}

#
# sleep is constant or read-only
# sort is constant or read-only.
# tee is constant or read-only.
#
foreach($cmd in 'b2sum', 'b3sum', 'base32', 'base64', 'basename', 'basenc', 'cat', 'cksum', 'comm', 'cp', 'csplit', 'cut', 'date', 'dd', 'df', 'dir', 'dircolors', 'dirname', 'du', 'echo', 'env', 'expand', 'expr', 'factor', 'false', 'fmt', 'fold', 'hashsum', 'head', 'join', 'link', 'ln', 'ls', 'md5sum', 'mkdir', 'mktemp', 'more', 'mv', 'nl', 'numfmt', 'od', 'paste', 'pr', 'printenv', 'printf', 'ptx', 'pwd', 'readlink', 'realpath', 'relpath', 'rm', 'rmdir', 'seq', 'sha1sum', 'sha224sum', 'sha256sum', 'sha3-224sum', 'sha3-256sum', 'sha3-384sum', 'sha3-512sum', 'sha384sum', 'sha3sum', 'sha512sum', 'shake128sum', 'shake256sum', 'shred', 'shuf', 'split', 'sum', 'tac', 'tail', 'test', 'touch', 'tr', 'true', 'truncate', 'tsort', 'unexpand', 'uniq', 'unlink', 'vdir', 'wc', 'yes') {
    Remove-Alias $cmd -ErrorAction SilentlyContinue
    Set-Item function:$cmd -Value {coreutils $cmd $args}.GetNewClosure()
}


# PSReadLine
Import-Module PSReadLine
# 设置预测性分析引擎,开启智能补全
Set-PSReadLineOption -PredictionSource History
# 设置补全热键,例如Tab
# Set-PSReadLineKeyHandler -Key Tab -Function Complete
#Tab键会出现自动补全菜单
Set-PSReadLineKeyHandler -Key Tab -Function MenuComplete 
# 上下方向键箭头，搜索历史中进行自动补全
Set-PSReadlineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadlineKeyHandler -Key DownArrow -Function HistorySearchForward

# oh-my-posh
# Import-Module oh-my-posh
# 主题设置
# oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\night-owl.omp.json" | Invoke-Expression
# （5.x语法）
# Set-PoshPrompt -Theme kali

# 文件图标
Import-Module -Name Terminal-Icons

# 自动补全
Import-Module PSCompletions

# 启动starship
Invoke-Expression (&starship init powershell)


# 自定义快捷命令

# 配置当前git项目用户名
Function github-u {
    git config --local user.name 'user'
    git config --local user.email 'example@gmail.com'
}

# 查看当前项目git设置
Function git-cl {
    git config --local --list
}

# 查看全局配置
Function git-gl {
    git config --global --list
}

# 设置当前项目git代理
Function git-lproxy {
    git config --local http.proxy 'socks5://127.0.0.1:10808'
    git config --local https.proxy 'socks5://127.0.0.1:10808'
}

# 设置全局git代理
Function git-proxy {
    git config --global http.proxy 'socks5://127.0.0.1:10808'
    git config --global https.proxy 'socks5://127.0.0.1:10808'
}

# 取消全局git代理
Function git-unproxy {
    git config --global --unset http.proxy
    git config --global --unset https.proxy
}

# 取消当前项目git代理
Function git-unlproxy {
    git config --local --unset http.proxy
    git config --local --unset https.proxy
}

```

## 🧰 [coreutils](https://github.com/uutils/coreutils)

安装：

```sh
# use cargo 
cargo install coreutils --features windows
# use scoop
scoop install uutils-coreutils
```

添加自动补全支持，参考[文章](https://next.cyp0633.icu/post/%E7%94%A8-rust-uutils-%E6%9B%BF%E6%8D%A2-windows-powershell-%E5%86%85%E7%BD%AE-cmdlet/)，或者直接下载
![[completions.zip]]