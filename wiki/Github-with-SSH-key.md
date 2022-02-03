Git for Windows 带有一个 "Git Bash", 它通过 SSH 为你提供良好的 Git 功能. 但如果你想使用 Powershell 而不是 Bash 怎么办? 本指南向你展示了如何做到这一点, **无需在每次连接时重新输入密码**.

本指南使用 Github 作为示例, 但相同的原则适用于任何 SSH 可访问的 Git 存储库.

假设你已经 [安装了 Scoop](https://github.com/lukesampson/scoop/wiki/Quick-Start), 且对 Git 有基本知识.

*基于 [来自 GitHub 的这个指南](https://help.github.com/articles/generating-ssh-keys#platform-windows)*

### 安装

首先, 安装所需的程序:

```command line
scoop install git openssh
```

### 创建一个私钥

如果你还没有 SSH 密钥, 可以像这样创建一个:

```powershell
PS> <b>ssh-keygen</b>
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/<b>you</b>//.ssh/id_rsa): <b>[press enter]</b>
Enter passphrase (empty for no passphrase): <b>[type your password]</b>
Enter same passphrase again: <b>[and once more]</b>
...
```

然后 [添加你的 SSH 密钥到 GitHub](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account).

### 用 Pshazz 记住你的密码

Pshazz 包含一个 SSH 插件, 可以将你的 SSH 密钥密码保存在 Windows 凭据管理器中, 因此无需在每次推送到 Github 存储库时都重新输入. 安装它:

```command line
scoop install pshazz
```

你可以通过以下方式设置 git 客户端以将你的 GitHub 访问令牌存储到 Windows 凭据管理器:

```command line
git config --global credential.helper manager
```

你应该会看到一个询问你的 SSH 密钥密码的弹出窗口: 输入它并选中该框以保存你的密码. 回到你的 Powershell 会话中, 你应该会看到 `Identity Added` 消息. 从现在开始, 无论何时启动 Powershell 会话, Pshazz 都会确保 `ssh-agent` 正在运行, 并使用你保存的密码加载你的私钥.

### 测试

要确认是否一切正常, 运行:

```command line
ssh -T git@github.com
```

一两次警告后, 你应该会看到这样的消息:

> Hi <username>! You've successfully authenticated, but GitHub does not provide shell access. 