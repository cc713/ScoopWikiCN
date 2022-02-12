应用程序的 `current` 目录是一个特别的 alias/别名 目录, 指向该应用程序当前安装的版本.

它允许路径引用在应用程序更新中保持不变, 因为路径引​​用的是 "current" 目录别名, 而不是硬编码的版本目录.

!['current' 别名如何工作](https://raw.githubusercontent.com/ScoopInstaller/scoop/gh-pages/images/Junctions%20Overview.png)

例如, 如果我现在运行 `ls ~/scoop/apps/git`, 我看到这个输出:

```powershell
$ ls ~/scoop/apps/git

    Directory: C:\Users\luke\scoop\apps\git


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         24/11/16   8:17 am                2.10.2.windows.1
d-----           3/1/17   9:42 am                2.11.0.windows.1
d----l           3/1/17   9:42 am                current
```

`2.10.2.windows.1` 和 `2.11.0.windows.1` 目录是安装的 Git 版本.

`current` 目录不是附加目录, 而是 **Directory Junction**/目录连接, 指向 2.11.0.windows.1 目录.

如果你密切注意, 你可能会注意到输出的 `Mode` 列中额外的 `l`. 但除此之外, 你不会看到太多迹象表明它与普通目录有任何不同.

## 什么是目录连接? ###

如果你不熟悉目录连接, 可以将它们视为类似于符号链接, 甚至是快捷方式. 它们是指向文件系统中其他位置的指针. Junction 和符号链接之间存在一些重要的实现差异, 如果感兴趣你可以阅读[这个](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365006(v=vs.85).aspx)

Scoop 使用 junction 而不是符号链接的原因是符号链接需要管理员权限才能创建
(虽然这 [看起来很快就会改变](https://blogs.windows.com/buildingapps/2016/12/02/symlinks-windows-10/#cpLA6xrKTwb5fOf7.97)).

### 为什么要使用 Junction？Shim 还不够吗? ###

这里要解决的主要问题是如何保持程序在更新之间顺利运行, 即使新程序与前一个程序位于不同的目录中. Shims 确实解决了这里的一些问题, 方法是留在同一个地方并更新它们指向的版本.

但是, 有些程序在安装后需要设置环境变量, 注册表设置或其他指向实际安装路径的配置. 在 Scoop 使用 `current` 目录连接之前, 这些变量和设置在升级后会指向旧目录, 这并不理想. 通过使用 `current` 别名目录并更新别名, 设置将继续指向正确的位置.

![为何用 Junction?](https://raw.githubusercontent.com/ScoopInstaller/scoop/gh-pages/images/Junctions%20Comparison.png)