## 数据目录

如果你想在更新间保留数据你应该使用 `~/scoop/persist/<app>/`.
在清单中, 指向数据目录的路径在 `$persist_dir` 变量中.

[PHP](/ScoopInstaller/Main/blob/master/bucket/php.json) 包将其用于配置文件.

## 应用清单

目录和文件可以添加到应用清单内的 "persist" 定义中.
persist 数据通过目录连接或硬链接从已安装的应用程序目录链接到数据目录.

在安装过程中, 任何保留数据都会复制到数据目录并被链接.

### 定义

`persist` 如果只需要一个项目, 则定义可以是字符串, 也可以是多个项目的数组.

可选地, 项目可以在数据目录中具有不同的名称

```json
{
    "persist": [
        "keeps_its_name",
        ["original_name", "new_name_inside_the_data_dir"]
    ]
}
```

### 例子

- [MySQL](/ScoopInstaller/Main/blob/master/bucket/mysql.json)
- [MariaDB](/ScoopInstaller/Main/blob/master/bucket/mariadb.json)
- [NGINX](/ScoopInstaller/Main/blob/master/bucket/nginx.json)
- [node.js](/ScoopInstaller/Main/blob/master/bucket/nodejs.json)
- [PHP](/ScoopInstaller/Main/blob/master/bucket/php.json)

## 卸载

卸载应用程序时, 有一个标志可以清除所有保留数据. 默认情况下, 数据将一直保留, 直到你将其删除.

```command line
scoop uninstall -p nodejs
```
