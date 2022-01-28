以下是一些示例 'get-all-my-stuff' 脚本. 

假设你有 Powershell 5 或以上且安装了 Scoop, 例如

```powershell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
set-executionpolicy unrestricted -s cu
```

### 示例开发环境设置

```powershell
# utils/实用工具

scoop install 7zip curl sudo git openssh coreutils grep sed less

# programming languages/编程语言

scoop install python ruby go nodejs

# WAMP stack/WAMP(Windows Apache MariaDB PHP) 栈

scoop install apache mariadb php
iex (new-object net.webclient).downloadstring('https://gist.github.com/lukesampson/6546858/raw/apache-php-init.ps1')

# console theme/控制台主题

scoop install concfg pshazz
concfg import solarized small

# vim

scoop install vim

'
set ff=unix

set cindent
set tabstop=4
set shiftwidth=4
set expandtab
set backupdir=$TEMP
' | out-file ~/.vimrc -enc oem -append

```

### 示例生产环境设置

```powershell
scoop install sudo 7zip

# make these available to system processes/让这些可用于系统进程

sudo scoop install git ruby postgres --global

# just for me/自用

scoop install grep coreutils 
```


