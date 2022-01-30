如果你想自定义 PHP 安装的设置, 永远不应该编辑 PHP 目录中的 `php.ini` 文件. 此文件 **不会** 在更新时保留!

始终在配置扫描目录 `%PHP_INI_SCAN_DIR%` 中创建你自己的自定义配置文件.

默认情况下, `%PHP_INI_SCAN_DIR%` 设置为 `~\scoop\apps\php\current\cli;~\scoop\apps\php\current\cli\conf.d;`.

- 你的自定义 `php.ini` 应放到 `~\scoop\apps\php\current\cli`
- 你可以将自定义 `.ini` 文件放到 `~\scoop\apps\php\current\cli\conf.d`
- 你可以创建尽可能多的 `.ini` 文件到 `~\scoop\apps\php\current\cli\conf.d`
- `~\scoop\apps\php\current\cli` 文件是到 `~\scoop\persist\php\cli` 的 symlink/符号链接, 它在更新时保留

## 一些示例 `.ini` 文件

### 自定义设置 `custom.ini`

一些基本设置, 例如时区和限制

```ini
date.timezone = Europe/Berlin
max_execution_time = 60
memory_limit = 256M
post_max_size = 128M
upload_max_filesize = 128M
```

### 调试 `debug.ini`

启用调试

```ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
html_errors = Off
```

#### 使用 Xdebug `xdebug.ini`

通过 `scoop bucket add extras` 和 `scoop install php-xdebug` 安装 Xdebug.

```ini
zend_extension=C:\Users\<user>\scoop\apps\php-xdebug\current\php_xdebug.dll
[xdebug]
xdebug.remote_enable=on
xdebug.remote_autostart=on
xdebug.remote_connect_back=on
```

### 启用 PHP 模块 `extensions.ini`

查看 `php.ini` 内部以了解可用内容.

```ini
extension_dir=ext

extension=bz2
extension=curl
extension=fileinfo
extension=gd2
;extension=gettext
;extension=gmp
extension=intl
;extension=imap
;extension=interbase
;extension=ldap
extension=mbstring
extension=exif      ; Must be after mbstring as it depends on it
;extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
;extension=odbc
extension=openssl
;extension=pdo_firebird
extension=pdo_mysql
;extension=pdo_oci
;extension=pdo_odbc
extension=pdo_pgsql
extension=pdo_sqlite
extension=pgsql
;extension=shmop

; The MIBS data available in the PHP distribution must be installed.
; See http://www.php.net/manual/en/snmp.installation.php
;extension=snmp

;extension=soap
;extension=sockets
;extension=sodium
extension=sqlite3
;extension=tidy
;extension=xmlrpc
;extension=xsl
```


### 设置 Curl 和 Openssl `cacert.ini`

通过 `scoop install cacert` 安装 `cacert.pem` 并配置 curl 和 openssl 以使用 `cacert.pem`.

```ini
[curl]
curl.cainfo="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
[openssl]
openssl.cafile="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
openssl.capath="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
```

_提示: 你可以使用 git 来存储你的配置_