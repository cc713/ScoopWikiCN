本指南将帮助你在 Windows 上使用 SSH 连接到 SSH 服务器. 你将获得与 SSH 在 MacOS 或 Linux 上的工作方式类似的体验. 不需要 PuTTy 或 GUI, 甚至可以对其进行设置, 这样你就不必在每次连接时重新输入私钥密码.

本指南假设你已 [安装 Scoop](https://github.com/lukesampson/scoop/wiki/Quick-Start) 并有一台运行 SSH 服务器的 Linux 机器 — 我们需要一些东西来连接. 它还假设你基本熟悉 [SSH 是干什么的](http://en.wikipedia.org/wiki/Secure_Shell) 只想知道怎么在 Windows 上使用它.

### 安装

> 如果你使用的是 Windows 10 版本 1803（2018 年 4 月）或更高版本, 你的系统中已经安装了内置的 win32-openssh 并已添加到系统 PATH 中. 可以运行 `scoop which ssh` 来定位你正在使用的 ssh, 并且可以选择跳过外部 openssh 安装.

首先, 从 Powershell 命令提示符安装 SSH:

```command line
scoop install openssh
```

P.S. 如果你想在 git 中使用 ssh, 你可能更喜欢通过 `scoop install git-with-openssh` 安装 `git-with-openssh`

或者, 对于最新版本的 openssh:

```command line
scoop install win32-openssh
```

### 使用密码通过 SSH 连接

假设你有一个在 `example.org` 运行的 Web 服务器. 你现在应该能够连接到它

```command line
ssh username@example.org
```

输入密码后, 你应该登录到远程服务器. 拍拍自己的背, 你已经从 Windows 连接了 SSH! 容易, 对吧?

密码很好, 但为了额外的安全性, 我们可以使用受密码保护的密钥. 让我们现在进行设置.

### 创建用于身份验证的密钥

如果你已经有私钥 (例如 ~/.ssh/id_rsa) 你可以跳过这步. 否则, 像这样创建一个新的私钥 (输入 **粗体** 文本):

<pre>
PS> <b>ssh-keygen</b>
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/username//.ssh/id_rsa): <b>[enter]</b>
Enter passphrase (empty for no passphrase): <b>your-super-secret-password</b>
Enter same passphrase again: <b>your-super-secret-password</b>
Your identification has been saved in /c/Users/username//.ssh/id_rsa.
Your public key has been saved in /c/Users/username//.ssh/id_rsa.pub.
The key fingerprint is:
d5:96:01:96:7a:63:25:93:a0:d6:65:0b:1a:a3:e7:05 username@COMPUTER
The key's randomart image is:
+--[ RSA 2048]----+
|      E o.=+.    |
|     . B ==o.o   |
|    . = o.o++    |
|     + ...+.     |
|      . So .     |
|                 |
|                 |
|                 |
|                 |
+-----------------+
</pre>

如果你使用上述默认文件, 你的私钥会被创建在 `~/.ssh/id_rsa` 你的公钥会在 `~/.ssh/id_rsa.pub`.

### 使用 SSH 密钥连接

在我们可以用 SSH 密钥连接到我们的服务器之前 (例子 `example.org`), 需要通过将我们的公钥复制到远程服务器来授权我们将使用的密钥:

```command line
cat ~/.ssh/id_rsa.pub | ssh username@example.org 'mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

现在尝试再次连接:

```command line
ssh username@example.org
```

这一次, 不是要求你输入 "用户名" 密码, 而是要求你输入私钥的密码.

### 使用 Pshazz 获得更好的 SSH 体验

现在, 每次重新启动 PC 并打开控制台会话时, 都需要手动启动 SSH 代理, 并且每次使用 `ssh username@example.org` 连接时, 系统都会要求你输入私钥密码.

你可以用 [Pshazz](https://github.com/lukesampson/pshazz) 自动启动 SSH 代理并缓存你的密钥密码.

```command line
scoop install pshazz
```

然后 Pshazz 将自动启动 SSH 代理并添加你的密钥. 第一次会要求你输入密钥密码. 尝试通过 SSH 连接:

```command line
ssh username@example.org
```

如果一切按计划进行, 你应该无需输入密码即可登录. 万岁!

要查看发生了什么, 请输入:

```command line
ssh-add -l
```

SSH 密钥的指纹应该会显示. `ssh-agent` 现在将在你使用 SSH 时尝试使用此密钥.

更重要的是, Pshazz 支持 `ssh` 命令的 tab 补全:

```command line
    ssh <TAB>
```

你将在 `~/.ssh/config` 看到所有主机.

你可能会注意到你的 SSH 会话超时. 为了防止这种情况, 我喜欢添加一个 ServerAliveInterval 到我的 ~/.ssh/config (如果此文件不存在, 你可能需要创建它):

```command line
Host *
    ServerAliveInterval 30
```

这将每 30 秒向远程服务器发送一个空数据包以保持连接处于活动状态.
