### 你需要这个吗?

如果你的代理已在 Internet 选项中配置且不需要身份验证, 则你无需为 Scoop 执行任何其他操作即可使用它.

这些说明适用于那些

1. 需要通过他们的代理进行身份验证, 使用他们的 Windows 凭据或其他用户名/密码
2. 想要为 Scoop 使用未在 Internet 选项中配置的代理服务器.

## 安装

通常, Scoop 通过以下命令安装

```powershell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

如果你使用代理, 则可能需要先运行以下一个或多个命令:

```
# If you want to use a proxy that isn't already configured in Internet Options
[net.webrequest]::defaultwebproxy = new-object net.webproxy "http://proxy.example.org:8080"

# If you want to use the Windows credentials of the logged-in user to authenticate with your proxy
[net.webrequest]::defaultwebproxy.credentials = [net.credentialcache]::defaultcredentials

# If you want to use other credentials (replace 'username' and 'password')
[net.webrequest]::defaultwebproxy.credentials = new-object net.networkcredential 'username', 'password'
```

这些命令将影响使用 `net.webclient` 的任何 Web 请求, 直到你的 powershell 会话结束.

## 配置 Scoop 以使用你的代理

安装 Scoop 后, 你可以使用 `scoop config` 来配置你的代理. 这是 `scoop help config`的摘录:

---
`scoop config proxy [username:password@]host:port`

默认情况下, Scoop 将使用 Internet 选项中的代理设置, 但使用匿名身份验证.

* 要使用当前登录用户的凭据, 请使用 `currentuser` 代替 `username:password`
* 要使用 Internet 选项中配置的系统代理设置, 用 `default` 代替 `host:port`
* 代理的空值或未设置值相当于 `default` (没有用户名或密码)
* 要绕过系统代理直接连接, 用 `none` (没有用户名或密码)

---

## 配置示例

##### 将你的 Windows 凭据与 Internet 选项中配置的默认代理一起使用

```powershell
scoop config proxy currentuser@default
```

##### 使用硬编码凭据和 Internet 选项中配置的默认代理

```powershell
scoop config proxy user:password@default
```

##### 使用未在 Internet 选项中配置的代理

```powershell
# anonymous authentication to proxy.example.org on port 8080:/在端口 8080 上对 proxy.example.org 进行匿名身份验证:

scoop config proxy proxy.example.org:8080

# or, with authentication:/或, 带认证:

scoop config proxy username:password@proxy.example.org:8080
```

##### 绕过 Internet 选项中配置的代理

```powershell
scoop config rm proxy
```

##### 使用包含 `@` 或 `:` 的密码

如果你的代理密码包含 `@` 或 `:` 字符, 则需要使用 `\` 对其进行转义, 例如:

```powershell
scoop config proxy 'username:p\@ssword@proxy.example.org:8080'
```

##### 对 "bucket add" 命令的代理

这是一个潜在的 bug, 但我找到了解决方法:

如果你尝试运行 `scoop config proxy currentuser@proxy.example.org:8080` 然后 `scoop bucket add extras` 它不起作用, 因为 scoop 将这些凭据直接传递给 git 且 git 具有不同的代理格式. 因此, 要使 "bucket add" 工作, 你必须将代理字符串更改为 git 理解的格式, 以便 `currentuser@proxy.example.org:8080 ` 变为 `:@proxy.example.org:8080 `

因此下面命令可用:

```powershell
scoop config proxy ':@proxy.example.org:8080'
scoop bucket add extras
```

但是你必须将代理字符串更改回正常才能安装实际软件包:

```powershell
scoop config proxy 'currentuser@proxy.example.org:8080'
scoop install vscode
```
