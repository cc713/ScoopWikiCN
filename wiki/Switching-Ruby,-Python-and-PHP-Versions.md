### `scoop reset`

如果你需要在同一台计算机上运行多个版本的 Ruby 或 Python, 可以使用 `scoop reset` 在版本之间切换.
如果你熟悉 rbenv 或 RVM, 这大致相似, 尽管使用 `scoop reset` 时总是需要手动切换版本.

### 例子

```powershell
scoop bucket add versions # add the 'versions' bucket if you haven't already/如果还没有, 添加 'version' 存储桶

scoop install ruby ruby19
ruby --version # -> ruby 1.9.3p551 (2014-11-13) [i386-mingw32]

# switch to ruby 2.x/切换到 ruby 2.x
scoop reset ruby
ruby --version # -> ruby 2.3.3p222 (2016-11-21 revision 56859) [x64-mingw32]

# switch back to 1.9.x/切换回 1.9.x
scoop reset ruby19
ruby --version # -> ruby 1.9.3p551 (2014-11-13) [i386-mingw32]
```

`scoop reset` 重新安装应用程序的 shim 并根据应用程序清单更新环境变量.
可以在 Ruby 版本之间切换任意多次—两个版本都保持安装状态, 但一个优先.

## Python

Python 类似

```powershell
scoop bucket add versions # add the 'versions' bucket if you haven't already

scoop install python27 python
python --version # -> Python 3.6.2

# switch to python 2.7.x/切换到 python 2.7.x
scoop reset python27
python --version # -> Python 2.7.13

# switch back (to 3.x)/ 切换回(3.x)
scoop reset python
python --version # -> Python 3.6.2
```

## PHP

```powershell
scoop bucket add versions # add the 'versions' bucket if you haven't already

scoop install php74 php
php --version # -> PHP 8.0.2

# switch to PHP 7.4.x/切换到 PHP 7.4.x
scoop reset php74
php --version # -> PHP 7.4.15

# switch back to latest version/切换回最新版本
scoop reset php
php --version # -> PHP 8.0.2
```