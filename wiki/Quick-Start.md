### 先决条件

要得到 PowerShell 命令提示符
  * "Start" --> (搜索) "cmd"
  * 终端窗口应该出现
  * "powershell"
  * 提示符开头为 "PS "

确保你安装了 **PowerShell 5.0** 或以上. 如果你在 *Windows 10* 或 *Windows Server 2012* 你应该准备好了, 但 *Windows 7* 和 *Windows Server 2008* 也许用旧版本.

```powershell
$psversiontable.psversion.major # should be >= 5.0
```

确保你已允许 PowerShell 执行本地脚本:

```powershell
set-executionpolicy remotesigned -scope currentuser
```

`Unrestricted` 也行, 但更不安全. 因此, 如果你不确定, 请坚持使用 `RemoteSigned`.

### 安装 Scoop

在 PowerShell 命令控制台, 运行:

```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

或更短的:

```powershell
iwr -useb get.scoop.sh | iex
```

### 安装 Scoop 到自定义目录

假设目标目录是 `C:\scoop`, 在 PowerShell 命令控制台, 运行:

```powershell
$env:SCOOP='C:\scoop'
[environment]::setEnvironmentVariable('SCOOP',$env:SCOOP,'User')
iwr -useb get.scoop.sh | iex
```

假设你没有看到任何错误消息, Scoop 现在可以运行了.

### 安装全局 app 到自定义目录

假设目标目录是 `C:\apps`, 在管理员权限打开的 PowerShell 命令控制台, 运行:

```powershell
$env:SCOOP_GLOBAL='c:\apps'
[environment]::setEnvironmentVariable('SCOOP_GLOBAL',$env:SCOOP_GLOBAL,'Machine')
scoop install -g <app>
```

### 使用 Scoop

尽管 Scoop 是用 PowerShell 写的, 与大多数 PowerShell 程序相比, 它的界面更接近 Git 和 Mercurial.

概览 Scoop 的界面, 运行:

```command line
scoop help
```

你将看到一个命令列表, 其中简要说明了每个命令的作用. 有关命令的详细信息, 运行 `scoop help <command>`, 例如 `scoop help install`(试试看!).

现在你已经大致了解了 Scoop 命令的工作原理, 让我们尝试安装一些东西.

```command line
scoop install curl
```

你可能会看到有关缺少哈希的警告, 但你应该会看到一条最终消息, 表明 cURL 已成功安装. 尝试运行它:

```command line
curl -L https://get.scoop.sh
```

你应该会看到一些 HTML, 可能带有 "document moved" 消息. 请注意, 就像安装 Scoop 时一样, 无需重启控制台即可使程序正常工作. 此外, 如果你在注意到没有收到关于 SSL 的错误之前手动安装了 cURL — Scoop 为你下载了一个证书包.

##### 查找 app

假设你想安装 `ssh` 命令, 但不确定在哪里可以找到它. 尝试运行:

```command line    
scoop search ssh
```

你应该会看到 "openssh" 的结果. 这是一个简单的案例, 因为应用程序的名称包含 "ssh".

你还可以通过它们安装的命令的名称来查找应用程序. 例如,

```command line
scoop search hg
```

这表明 "mercurial" 应用程序包含 "hg.exe".

### 更新 Scoop

要获取最新版本的 Scoop, 你必须运行命令

```command line
scoop update
```

这将下载最新版本的 scoop 并更新本地应用程序清单.

更新 Scoop 后, 你可以更新个别应用

```command line
scoop update curl
```

要更新所有已安装的应用程序, 可运行

```command line
scoop update *
```
