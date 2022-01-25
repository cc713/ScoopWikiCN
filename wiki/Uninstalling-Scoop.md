如果你尝试过 Scoop, 但它不适合你 — 没问题. 你可以通过运行以下命令卸载 Scoop 以及你使用 Scoop 安装的所有程序:

```command line
scoop uninstall scoop
```

这会让你知道会发生什么, 并询问你是否确定 — 只需输入 'y' 并按 Enter 键确认.

这个命令会删除 `~/scoop/persist` 文件夹.

### 损坏的安装

如果你删除 `~/scoop` 你应该可以重新安装. 在 powershell 运行:

```
del .\scoop -Force
```
