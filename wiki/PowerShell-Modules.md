PowerShell modules/模块像其它 app 一样安装, 但它们也在 `~\scoop\modules` 下被链接.

`~\scoop\modules` 目录会被添加到你的 `$env:PSModulePath` 环境变量, 且 PowerShell 应该会自动检测你使用 Scoop 安装于此的 modules/模块.

`~\scoop\modules` 下的目录不是通常目录. 每个都是 **directory junction**/目录连接, 指向当前安装的 app/module 版本, 它本身就是一个指向实际版本化目录的目录连接. 因此对名为 `MyPSModule` 的 module, 你可能会看到:

~\scoop\modules\MyPSModule
  → 指向 ~\scoop\apps\mypsmodule\current
    → 指向 ~\scoop\apps\mypsmodule\1.16.0.rc2

用于 PowerShell module 的 [Scoop manifest](../concepts/App-Manifests.md) 关键部分是:

```json
{
  "psmodule": {
    "name": "NameOfTheModule"
  }
}
```

如果你使用 `psmodule`, `name` 属性是必需的, 它应该与 PowerShell 模块的 `.psd1` manifest 名称匹配, 以便 PowerShell 认为它 "格式正确" 并自动检测 module (详情见 [此](https://msdn.microsoft.com/en-us/library/dd878350(v=vs.85).aspx).)

