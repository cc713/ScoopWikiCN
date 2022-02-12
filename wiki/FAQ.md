*你有在这里没有回答的问题?请创建一个 issue.*

### 如何更新我的 app?

首先, 更新 Scoop 以得到最新清单:

```
scoop update
```

然后更新 app, 例如 Git:

```
scoop update git
```

如果你想一次更新所有应用, 可以使用通配符 '*':

```
scoop update *
```

### 如何安装特定版本的应用程序?

如果你需要安装特定版本的应用程序, 用 `scoop install [app]@[version]`. 例如 对于 Git:

```
scoop install git@2.23.0.windows.1
```

### 我可以使用 scoop 安装特定应用程序的多个版本吗?

你可以通过 install 命令使用 `@[version]` 语法安装多个版本:

```
scoop install git@2.19.0.windows.1

scoop install git@2.23.0.windows.1
```

当有新版本可用时, 你还可以通过运行 `scoop update` 来安装新版本的应用程序: 旧版本将一直存在, 直到你使用 `scoop cleanup` 命令将其删除.

**请注意:** 运行 `scoop list` 或 `scoop info` 会显示安装的最新版本, 而非全部.

### 如何在一个 app 的不同版本间切换?

用 `scoop reset [app]@[version]`. 例如 对于 terraform:

```
scoop reset terraform@0.11.14
```

将 terraform 的活动安装设置为版本 0.11.14.

```
scoop reset terraform@0.12.11
```

将 terraform 的活动安装设置为版本 0.12.11.

```
scoop reset terraform
```

将 terraform 的活动安装重置为 *最新安装的版本*.

**请注意:** `scoop reset` 仅允许你在已安装的版本之间切换.

### 如何卸载 app?

用 `scoop uninstall [app]`. 例如 对于 Git:

```
scoop uninstall git
```

### Scoop 安装时非常缓慢, 锁定 CPU, 或显示 access denied errors

在提取文件时, 你的防病毒或反恶意软件程序可能正在执行实时扫描. 更多信息和可能的解决方法参见 [Antivirus and Anti-Malware Problems](./Antivirus-and-Anti-Malware-Problems.md).

*[todo: 在这插入更多 FAQ]*
