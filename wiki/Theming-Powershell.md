![](https://github.com/ScoopInstaller/scoop/raw/gh-pages/images/docs/shell-theme.png)

这就是我命令行的外观, 在内置的 Windows 控制台中运行 Powershell. 你可以看看 [Solarized](http://ethanschoonover.com/solarized) 颜色主题, 和一个定制的包含 Git 信息的提示符. 你看不到 Git tab 补全或对 SSH 密钥的支持, 但这些也存在.

我为字体和颜色主题使用了 [Concfg](https://github.com/ScoopInstaller/concfg), 为定制提示符和 Git 和 SSH 功能使用了 [Pshazz](https://github.com/ScoopInstaller/pshazz).

这是我获取此设置的脚本:

```powershell
scoop install 7zip git openssh concfg

# back-up current console settings
concfg export console-backup.json

# use solarized color theme
concfg import solarized-dark

# You'll see this warning:
#     overrides in the registry and shortcut files might interfere with
#     your concfg settings.
#     would you like to search for and remove them? (Y/n):
# Enter 'n' if you're not sure yet: you can always run 'concfg clean' later

scoop install pshazz
```

如果你安装了 Pshazz 并且你已经有一个 SSH 密钥, 你会看到一个弹出窗口询问你的密码.
![](https://github.com/ScoopInstaller/scoop/raw/gh-pages/images/docs/askpass.png)

如果你感兴趣, 这里有 [更多关于用 Scoop 设置 SSH 的信息](https://github.com/ScoopInstaller/scoop/wiki/SSH-on-Windows).

现在你应该有一个更好看的命令提示符, 并带有一些有用的 Git 和 SSH 增强功能. 如果你想进一步自定义提示符, 检查 Github 上的 [Concfg](https://github.com/ScoopInstaller/concfg) 和 [Pshazz](https://github.com/ScoopInstaller/pshazz) 项目.

值得指出的是, concfg 也可以在旧 cmd.exe 中工作, 但使用 Powershell, 你可以获得额外的提示符增强功能, 以及触手可及的出色动态和函数式编程语言.

如果你不喜欢新的颜色主题并想返回, 请运行 `concfg import console-backup.json`. 如果你启动一个新控制台并发现你的颜色设置消失了, 请重新运行 `concfg import solarized small`, 并在询问你是否要清理注册表设置时输入 'y'.
