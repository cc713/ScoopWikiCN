## 变量

这些变量可用于 `pre_install` / `post_install` 脚本:

| 变量        | 示例                                      | 描述      |
|-----------------|----------------------------------------------|------------------|
| **非路径:**  | |
| `$app`          | `exampleapp`                                 | 应用名 (manifest 文件名) 
| `$architecture` | `64bit` or `32bit`                           | 要安装的 app CPU 架构
| `$cfg`          | `{SCOOP_BRANCH, SCOOP_REPO, lastupdate, etc}`| Scoop 配置 (powershell 对象)
| `$global`       | `$false` or `$true`                          | 对于全局安装/卸载为 `$true`    
| `$manifest`     | `@{homepage=https://example.com/; description=Example app; version=2.4.1; url=http://example.com/app-setup.exe;...` | 反序列化清单 (powershell 对象) 
| `$version`      | `1.2.3`                                      | 要安装的版本
| **重要路径:**  | |
| `$dir`          | `C:\Users\username\scoop\apps\$app\current` <br/>`C:\ProgramData\scoop\apps\$app\current` (global installs) |
| `$persist_dir`  | `C:\Users\username\scoop\persist\$app`<br/>`C:\ProgramData\scoop\persist\$app` (global installs) |
| **其它路径:**  | |
| `$bucketsdir`   | `C:\Users\username\scoop\buckets`            | 
| `$cachedir`     | `C:\Users\username\scoop\cache`              | 
| `$cfgpath`      | `~/.scoop`                                   | Scoop 配置的路径
| `$globaldir`    | `C:\ProgramData\scoop`                       | 通常是 `%ProgramData\scoop`, `%SCOOP_GLOBAL%` 会覆盖)
| `$modulesdir`   | `C:\Users\username\scoop\modules`            |  
| `$original_dir` | `C:\Users\username\scoop\apps\$app\1.2.3`    | 
| `$oldscoopdir`  | `C:\Users\username\AppData\Local\scoop`      | 
| `$scoopdir`     | `C:\Users\username\scoop`                    | 基本 Scoop 安装目录 (通常是 `%USERPROFILE%\scoop`, `%SCOOP% 会覆盖)

(_详情检查 [`lib/install`](https://github.com/lukesampson/scoop/blob/master/lib/install.ps1) 脚本_)

## 函数

### `appdir`

引用另一个 scoop 应用程序. 例如, 要检查是否安装了另一个应用程序, 可以使用:

`"post_install": [ "if (Test-Path \"$(appdir otherapp)\\current\\otherapp.exe\") { <# .. do something .. #> }"`

