### 什么是 buckets?

在 Scoop 中, buckets 是 app 的集合. 或者, 更具体地说, bucket/存储桶 是一个 Git 存储库, 其中包含描述如何安装应用程序的 JSON [应用清单](App-Manifests).

Scoop 有一个与 Scoop 捆绑在一起的 [main bucket](https://github.com/ScoopInstaller/Main/tree/master/bucket), 它始终可用作安装应用程序的主要来源.

默认情况下, 当你运行 `scoop install <app>` 时, 它会在 main bucket 中查找, 但也可以从其他 bucket 安装.

有一个可选的 [extras bucket](https://github.com/ScoopInstaller/scoop-extras) 包含不太符合 [main bucket 标准](https://github.com/ScoopInstaller/scoop/wiki/Criteria-for-including-apps-in-the-main-bucket)的应用程序, 但仍然很好. 还有一个可选的 [versions](https://github.com/ScoopInstaller/Versions) bucket, 其中包含一些知名软件包的旧版本.

并且 Scoop 支持添加其他 buckets. 任何人都可以使用自己的应用集设置自己的 bucket, 其他人可以从该 bucket 添加和安装 — 他们只需要知道 bucekt 的 Git 存储库的位置.

### Known buckets/已知存储桶

社区有一个 known buckets 列表, 可以在 [`buckets.json`](https://github.com/ScoopInstaller/scoop/blob/master/buckets.json) 中查看, 要查看 known buckets 列表执行:

```command line
scoop bucket known
```

### 从其它 buckets 安装

如果要从 main 以外的 bucket 安装, 则需要配置 Scoop 以了解 bucket. 例如, 要添加可选的 "extras" bucket, 请运行:

```command line
scoop bucket add extras
```

'extras' 桶是一个 [特殊 bucket](https://github.com/ScoopInstaller/scoop/blob/master/buckets.json), 因为它是 "known", 即 Scoop 已经知道这个 bucket 在哪里, 不必指定其位置.

假设 extras bucket 并非 known, 你添加它的方式是:

```command line
scoop bucket add extras https://github.com/ScoopInstaller/scoop-extras.git
```

也就是,

```command line
scoop bucket add <name-of-bucket> <location-of-git-repo>
```

你可以运行 `scoop help bucket` 以获取有关 bucket 的更多信息

### 创建你自己的 bucket

#### 使用模板

你可以使用 [BucketTemplate](https://github.com/ScoopInstaller/BucketTemplate) 存储库为 bucket 创建模板. 只需单击 `Use this template` 按钮. 为你的 bucket 选择一个名称, 按照自述文件底部的说明进行操作, 一切顺利. 该模板有一堆 GitHub Actions 和 AppVeyor 脚本, 这有几个优点:
- **持续集成**: 每个新的提交都会有 Scoop 的测试针对它运行, 以保持文件的格式和可读性.
- **持续交付**: 自动化工作流程每 4 小时运行一次, 它将存储桶中的所有清单更新为最新版本.
- **Issue 处理**: 自动处理和纠正 404 错误和哈希校验失败等常见问题.
- **Pull Request 处理**: 通过对必需属性, 哈希验证, CI 测试等运行检查来验证 Pull request.

#### 手动

下面是一个示例, 你可以使用 GitHub 托管它来创建新 bucket. 不过, 不必使用 GitHub — 可以使用你喜欢的任何源代码控制存储库, 甚至只是本地或网络驱动器上的 Git 存储库.

```powershell
git clone https://github.com/<your-username>/my-bucket
cd my-bucket
'{ version: "1.0", url: "https://gist.github.com/ScoopInstaller/6446238/raw/hello.ps1", bin: "hello.ps1" }' > hello.json
git add .
git commit -m "add hello app"
git push
```

#### bucket 创建后

1. 配置 scoop 使用新的 bucket:

```powershell
scoop bucket add my-bucket https://github.com/<your-username>/my-bucket
```

2. 检查它是否工作:

```powershell
scoop bucket list # -> you should see 'my-bucket'
scoop search hello # -> you should see hello listed under, 'my-bucket bucket:'
scoop install hello
hello # -> you should see 'Hello, <windows-username>!'
```

3. 要共享你的 bucket, 只需告诉人们如何添加你的 bucket, 即通过运行步骤 3 中的命令. 如果你希望你的 bucket 在 [Scoop Directory](https://github.com/rasa/scoop-directory) 中列出, 将 `scoop-bucket` topic 添加到其 github 页面.
