关于 Scoop 命令的信息是内置的. 如果你使用 Git 应该会发现帮助界面很熟悉.

要查看命令列表, 运行:

```command line
scoop help
```

要查看特定命令的帮助, 运行:

```command line
scoop help <command>
```

当前命令是 (`scoop help` 的输出):

```
alias          Manage scoop aliases\\管理 scoop alias/别名
bucket         Manage Scoop buckets\\管理 Scoop buckets/存储桶
cache          Show or clear the download cache\\显示或清除下载缓存
checkup        Check for potential problems\\检查潜在问题
cleanup        Cleanup apps by removing old versions\\通过删除旧版本清理应用
config         Get or set configuration values\\获取或设置配置值
create         Create a custom app manifest\\创建自定应用清单
depends        List dependencies for an app\\列出 app/应用的依赖
export         Exports (an importable) list of installed apps\\导出(可导入)已安装应用清单
help           Show help for a command\\显示某个命令的帮助
hold           Hold an app to disable updates\\保持 一个 app 禁用更新
home           Opens the app homepage\\打开 app 主页
info           Display information about an app\\显示关于一个 app 的信息
install        Install apps\\安装应用
list           List installed apps\\列出已安装应用
prefix         Returns the path to the specified app\\返回指定应用的路径
reset          Reset an app to resolve conflicts\\重置应用以解决冲突
search         Search available apps\\搜索应用
status         Show status and check for new app versions\\显示状态和检查新的应用版本
unhold         Unhold an app to enable updates\\解除保持以启用更新
uninstall      Uninstall an app\\卸载应用
update         Update apps, or Scoop itself\\更新应用或 Scoop 自身
virustotal     Look for app's hash on virustotal.com\\在 virustotal.com 查找应用哈希
which          Locate a shim/executable (similar to 'which' on Linux)\\定位 shim/可执行文件(类似 Linux 上的 'which')
```