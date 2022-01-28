_文章存根. 请按需要填写/修正. 也许这已经或更好地记录在其他地方, 那时删除/重定向._

对于一般理解和故障排除, 了解 Scoop 使用哪些文件夹以及它们在哪里会很有帮助. 这些是示例, 配置选项可能会更改它们在你机器上的位置.

`%USERPROFILE%\scoop` - 每个用户根位置 (默认)  
`%SCOOP_GLOBAL%` - 为所有用户安装的应用程序的根位置, `%SYSTEMDRIVE%\ProgramData\scoop`  
  
`...\scoop\buckets` - 可安装 app 的 manifest (也是 bucket 源存储库的 git clone/克隆)  
`...\scoop\cache` - 下载的安装程序
`...\scoop\persist` - ...?  
`...\scoop\shims` - 添加到 PATH, 指向已安装应用程序的 wrapper/包装器  

`%USERPROFILE%\.config\scoop` - _(?) 似乎是定义主 Scoop 存储库的网络路径的地方_




