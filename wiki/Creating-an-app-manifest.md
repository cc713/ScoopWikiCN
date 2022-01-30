如果你想安装 Scoop 中未包含的程序, 你可以轻松创建 [app manifest](App-Manifests).

### 一个基本例子

以下是创建和安装说 hello 的 'app' 的清单, 只用数行 powershell.

```powershell
# write an app manifest to hello.json
'{ "version": "1.0", "url": "https://gist.github.com/lukesampson/6446238/raw/hello.ps1", "bin": "hello.ps1" }' > hello.json

# install the app
scoop install hello

# did it work?
hello # -> should output 'Hello, <your-username>!'
```

### 分享你的 app

##### 在你的网络上共享

如果你希望网络上的其他人能够从你的应用清单安装, 可以将其放在网络共享位置, 例如 \\shared\files\scoop\hello.json. 然后, 为了让其他人安装你的应用程序, 你可以告诉他们运行:

```command line
scoop install \\shared\files\scoop\hello.json
```

##### 与世界分享

如果你在网络上公开你的应用清单, 任何人只要知道 URL 就可以安装它. 例如, 我为 hello.json 制作了一个 GitHub gist 在 [这](https://gist.github.com/lukesampson/6446567). 现在任何人都可以安装它:

```command line
scoop install https://gist.github.com/lukesampson/6446567/raw/hello.json
```

### 下一步

如果你运行其中一些示例, 可能会注意到一条警告说 'no hash in manifest'. 有关在清单中指定文件 hash 等的参考信息, 请参阅 [App Manifests reference](App-Manifests).

如果你想维护应用程序集合, 请参阅[Buckets](Buckets)页面了解更多信息.
