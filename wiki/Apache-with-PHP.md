安装 PHP 和 Apache:

```command line
scoop install php apache
```

向 Apache 注册 PHP 处理程序:

```powershell
iex (new-object net.webclient).downloadstring('https://gist.githubusercontent.com/nilkesede/c98a275b80b6d373131df82eaba96c63/raw/apache-php-init.ps1')
```

**要从命令行启动 Apache**, 运行:

```command line
httpd
```

Apache 会在你按 `Ctrl-C` 中止前一直运行.

如果你在浏览器打开 http://localhost, 你应该看到一个页面写着 &ldquo;It works!&rdquo;.

## 文档根目录

Scoop 配置 Apache 从 Scoop 安装目录下的 htdocs 目录提供网页.

你可以运行以下命令到这个目录:

```command line 
pushd "$(scoop which httpd | split-path)\..\htdocs"
```

如果你想从其他地方提供文档, 你需要改变 conf/httpd.conf 文件中的 DocumentRoot. 你可以通过以下命令找到 httpd.conf

```command line
"$(scoop which httpd | split-path)\..\conf\httpd.conf"
```

## 安装 Apache 为服务

运行:

```command line
sudo httpd -k install -n apache
sudo net start apache
```

如果你没有 `sudo`, 你可以通过 `scoop install sudo` 安装.

要卸载 Apache 服务

```command line
sudo net stop apache
sudo httpd -k uninstall -n apache
```

更多信息, 参见 [在 Windows 上使用 Apache HTTP 服务器](http://httpd.apache.org/docs/current/platform/windows.html).
