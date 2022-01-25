一个 app manifest/应用清单是一个描述如何安装程序的 JSON 文件.

**一个简单的例子**:

```json
{
    "version": "1.0",
    "url": "https://github.com/lukesampson/cowsay-psh/archive/master.zip",
    "extract_dir": "cowsay-psh-master",
    "bin": "cowsay.ps1"
}
```

当这个清单用 `scoop install` 运行时, 它会下载 `url` 指定的 zip 文件, 提取 "cowsay-psh-master" 目录, 并让 `cowsay.ps1` 脚本在你的路径中可用.

更多例子, 见 [main Scoop bucket](https://github.com/ScoopInstaller/Main/tree/master/bucket) 中的应用清单.

### 必需属性

- `version`: 清单安装的应用版本.
- `description`: 包含程序简短描述的单行字符串. 不要包含程序的名称, 如果它与应用程序的文件名相同.
- `homepage`: 程序的主页.
- `license`: 程序软件许可证的字符串或哈希. 对于知名许可证, 请使用 <https://spdx.org/licenses> 中的标识符. 对于其他许可证, 请使用许可证的 URL(如果有). 否则, 请酌情使用 "Freeware"("免费软件"), "Proprietary"("专有软件"), "Plublic Domain"("公共领域"), "Shareware"("共享软件") 或 "Unknown"("未知"). 如果不同文件有不同的许可证, 请使用逗号 (,) 分隔许可证. 如果整个应用程序是 [dual licensed](https://en.wikipedia.org/wiki/Multi-licensing)/双重许可, 请使用竖线符号 (|) 分隔许可.

  - `identifier`: SPDX 标识符, 或 "Freeware"("免费软件"), "Proprietary"("专有软件"), "Plublic Domain"("公共领域"), "Shareware"("共享软件") 或 "Unknown"("未知"), 视情况而定.
  - `url`: 对于非 SPDX 许可证, 请包含指向该许可证的链接. 也可以包含指向 SPDX 许可证的链接.
  - 如果难以确定所有不同的许可证, 可以在 SPDX 列表的末尾添加 `,...`. 如果同时存在开源和非开源许可证, 请首先列出所有非开源许可证(例如, Freeware/Proprietary/Shareware).
  - 如果你无法确定应用程序拥有什么许可证, 请使用 "Unknown".

### 可选属性

- `##`: 包含注释的单行字符串或字符串数​​组.
- `architecture`: 如果应用程序具有 32 位和 64 位版本, 则可以使用 architecture 来包装差异 ([例子](https://github.com/ScoopInstaller/Main/blob/master/bucket/7zip.json)).
  - `32bit|64bit`: 包含特定于架构的指令 (`bin`, `checkver`, `extract_dir`, `hash`, `installer`,  `pre_install`, `post_install`, `shortcuts`, `uninstaller`, `url`, 和 `msi` [`msi` 已废弃]).
- [`autoupdate`](App-Manifest-Autoupdate#adding-autoupdate-to-a-manifest): 定义清单怎样自动更新.
- `bin`: 要在用户路径上可用的程序(可执行文件或脚本)字符串或字符串数​​组.
  - 你还可以创建一个别名 shim, 它使用与实际可执行文件不同的名称并(可选)将参数传递给可执行文件. 而不是仅仅为可执行文件使用一个字符串, 用例: `[ "program.exe", "alias", "--arguments" ]`. 例子见 [busybox](https://github.com/ScoopInstaller/Main/blob/master/bucket/busybox.json).
  - 但是, 如果你像这样声明一个 shim, 则必须确保它包含在外部数组中, 例如:
      `"bin": [ [ "program.exe", "alias" ] ]`. 否则它将被读取为多个单独 shim.
- [`checkver`](App-Manifest-Autoupdate#adding-checkver-to-a-manifest): App 维护者和开发者可使用 [bin/checkver](https://github.com/lukesampson/scoop/blob/master/bin/checkver.ps1) 工具检查 app 的更新版本. 清单中的 `checkver` 属性是一个正则表达式, 可用于匹配应用主页中应用的当前稳定版本. 例如, 见 [go](https://github.com/ScoopInstaller/Main/blob/master/bucket/go.json) 清单. 如果主页没有当前版本的可靠指示, 你还可以指定不同的 URL 进行检查 — 例如见 [ruby](https://github.com/ScoopInstaller/Main/blob/master/bucket/ruby.json) 清单.
- `depends`: 将自动安装的应用程序的运行时依赖项. 另请参阅 `suggest` (下文)以获取 `depends` 的替代方案.
- `env_add_path`: 将此目录添加到用户路径(或系统路径, 如果使用了 `--global`). 该目录是相对于安装目录的, 并且必须在安装目录中.
- `env_set`: 为用户(或系统, 如果使用 `--global`)设置一个或多个环境变量([示例](https://github.com/ScoopInstaller/Main/blob/master/bucket/go.json)).
- `extract_dir`: 如果 `url` 指向压缩文件(支持 .zip, .7z, .tar, .gz, .lzma 和 .lzh), Scoop 将仅提取其中指定的目录.
- `extract_to`: 如果 `url` 指向一个压缩文件(支持 .zip, .7z, .tar, .gz, .lzma 和 .lzh), Scoop 会将所有内容提取到指定目录([例子](https://github.com/lukesampson/scoop-extras/blob/master/bucket/irfanview.json)).
- `hash`: `url` 中每个 URL 的文件哈希的字符串或字符串数​​组. 哈希默认为 SHA256, 但你可以使用 SHA512, SHA1 或 MD5, 方法是在哈希字符串前面加上 "sha512:", "sha1:" 或 "md5:".
- `innosetup`: 如果安装程序基于 InnoSetup, 将此值设为布尔值 `true` (无引号).
- `installer`: 运行非 MSI 安装程序的指令.
  - `file`: 安装程序可执行文件. 对于 `installer` 默认为最后下载的 URL. 必须为 `uninstaller` 指定.
  - `script`: 要作为安装程序/卸载程序而不是 "文件" 执行的命令, 单行字符串或字符串数​​组.
    - `args`: 要传递给安装程序的参数数组. 可选.
  - `keep`: `"true"` 安装程序是否应该在运行后保留(例如, 以备将来卸载). 如果省略或设置为任何其他值, 安装程序将在运行后被删除. 例子见 [`extras/oraclejdk`](https://github.com/lukesampson/scoop-extras/blob/master/oraclejdk.json). 在 `uninstaller` 指令中使用此选项时将被忽略.
  - `script` 和 `args` 可用的变量: `$fname` (上次下载的文件), `$manifest` (反序列化的清单引用), `$architecture` (`64bit` 或 `32bit`), `$dir`(安装目录)
  - 在 `scoop install` 和 `scoop upgrade` 时都调用.
- `notes`: 单行字符串或字符串数​​组, 在安装应用程序后显示一条消息.
- `persist` 保存在应用程序数据目录中的目录和文件的字符串或字符串数​​组. [Persistent data](Persistent-data)
- `post_install`: 安装应用程序后要执行的命令, 单行字符串或字符串数​​组. 可以使用像 `$dir`, `$persist_dir` 和 `$version` 这样的变量. 有关详细信息, 请参阅 [安装前和安装后脚本](Pre--and-Post-install-scripts).
- `pre_install`: 与 `post_install` 相同的选项, 但在安装应用程序之前执行.
- `psmodule`: 安装为 `~/scoop/modules` 中的 PowerShell module/PowerShell 模块.
  - `name` (`psmodule` 必需): 模块的名称, 它应该与提取目录中的至少一个文件匹配, 以便 PowerShell 将其识别为模块.
- `shortcuts`: 指定在开始菜单中可用的快捷方式. 例子见 [sourcetree](https://github.com/lukesampson/scoop-extras/blob/master/bucket/sourcetree.json). 该数组必须包含一个可执行文件/标签对. 第 3,4 个元素是可选的.
  1. 到目标文件的路径 [必需]
  1. 快捷方式的名称(支持子目录: `<AppsSubDir>\\<AppShortcut>` 例如 [sysinternals](https://github.com/lukesampson/scoop-extras/blob/master/bucket/sysinternals.json))[必需]
  1. 启动参数 [可选]
  1. 图标文件路径 [可选]
- `suggest`: 显示一条消息, 建议提供补充功能的可选应用程序. 例子见 [ant](https://github.com/ScoopInstaller/Main/blob/master/bucket/ant.json).
  - `["Feature Name"] = [ "app1", "app2"... ]`
  例子 `"JDK": [ "extras/oraclejdk", "openjdk" ]`
如果已经安装了针对该功能建议的任何应用程序, 则该功能将被视为 "已满足", 用户将看不到任何建议.
- `uninstaller`: 与 `installer` 相同的选项, 但运行文件/脚本来卸载应用程序.
  - 在 `scoop uninstall` 和 `scoop upgrade` 时都调用.
- `url`: 要下载的文件的 URL. 如果有多个 URL, 可以使用 JSON 数组，例如 `"url": [ "http://example.org/program.zip", "http://example.org/dependencies.zip" ]`. URL 可以是 HTTP, HTTPS 或 FTP.
  - 要更改下载 URL 的文件名, 可以将 URL 片段(以 `#` 开头)附加到 URL. 例如,
  - `"http://example.org/program.exe"` -> `"http://example.org/program.exe#/dl.7z"`
  - 注意, 片段必须以 `#/` 开头才能正常工作.
  - 在上面的示例中, Scoop 将下载 `program.exe`, 但将其保存为 `dl.7z`, 然后使用 7-Zip 自动解压缩. 这种技术通常在 Scoop 清单中用于绕过可执行安装程序, 这些安装程序可能会产生不良副作用, 例如注册表更改, 放置在安装目录之外的文件或管理员权限提示.

### 未编档属性

- `cookie`: 仅见于 [此](https://github.com/se35710/scoop-java/search?q=cookie&unscoped_q=cookie)

### 废弃属性

- `_comment`: 包含注释的单行字符串或字符串数​​组. 用 `##` 代替.
- `msi` *(已废弃)*: 运行 MSI 安装程序的设置
**此属性已弃用, 并且支持将在 Scoop 的未来版本中删除.**- *新方法是将 .msi 文件视为 .zip 并从中提取文件, 而无需运行完整安装. 可以通过不在清单中包含此 `msi` 属性来使用新方法.*
  - `code` *必需*: MSI 安装程序的产品代码 GUID
  - `silent`: 通常应该是 `true` 以尝试在没有弹出窗口和 UAC 提示的情况下安装
