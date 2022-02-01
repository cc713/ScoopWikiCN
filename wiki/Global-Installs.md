默认情况下, Scoop 仅为你的用户帐户安装应用程序. 应用程序安装在 ~\scoop\apps 下, 并且只修改你的环境变量. 这通常很好, 尤其是在你的开发环境中.

在某些情况下, 你可能希望在系统范围内安装应用程序, 以便其他用户(包括本地系统帐户)可以访问它. Scoop 提供了一个 `--global` 开关来支持这种情况.

全局安装需要管理员权限, 因为它们安装到 \ProgramData\scoop, 并设置系统环境变量. 出于这个原因, 这个例子使用了 `sudo` 命令, 它大致相当于 UNIX 命令来运行具有超级用户权限的命令. 要安装它, 运行:

```command line
scoop install sudo
```

此外, 你可以用管理员身份运行打开一个普通的 Powershell 控制台.

### 示例

要安装一个 app:

```command line
sudo scoop install git --global
```

*注意：如果你希望应用程序可用于本地系统帐户, 则需要在全局安装第一个应用程序后重新启动系统. 这是因为在重新启动 Windows 之前, 对环境变量所做的更改不会对本地系统帐户生效 (见 [此](http://support.microsoft.com/kb/821761)).*

你也可以使用 `--global` 的缩写形式, `-g`.

例如, 使用简写形式更新全局安装的应用程序:

```command line
sudo scoop update git -g
```
