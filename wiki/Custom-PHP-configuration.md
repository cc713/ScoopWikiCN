If you want to customize the settings of your PHP Installation you should never edit the `php.ini` file inside the PHP directory. This file will **not** survive updates!

Always create your own custom configuration files inside the configuration scan directory `%PHP_INI_SCAN_DIR%`.

By default `%PHP_INI_SCAN_DIR%` is set to `~\scoop\apps\php\current\cli;~\scoop\apps\php\current\cli\conf.d;`.

- Your custom `php.ini` should be put into `~\scoop\apps\php\current\cli`
- You can put custom `.ini` files into `~\scoop\apps\php\current\cli\conf.d`
- You can create as many `.ini` files in `~\scoop\apps\php\current\cli\conf.d` as you like
- The `~\scoop\apps\php\current\cli` directory is a symlink to `~\scoop\persist\php\cli`, so it survives updates

## Some examples `.ini` files

### Custom settings `custom.ini`
Some basic settings like the timezone and limits

```ini
date.timezone = Europe/Berlin
max_execution_time = 60
memory_limit = 256M
post_max_size = 128M
upload_max_filesize = 128M
```

### Debugging `debug.ini`
Enabling debugging

```ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
html_errors = Off
```
#### Using Xdebug `xdebug.ini`
Install Xdebug via `scoop bucket add extras` and `scoop install php-xdebug`.
```ini
zend_extension=C:\Users\<user>\scoop\apps\php-xdebug\current\php_xdebug.dll
[xdebug]
xdebug.remote_enable=on
xdebug.remote_autostart=on
xdebug.remote_connect_back=on
```

### Enabling PHP modules `extensions.ini`
Take a look inside the `php.ini` to know what is available.

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


### Setup Curl and Openssl `cacert.ini`
Install `cacert.pem` via `scoop install cacert` and configure curl and openssl to use the `cacert.pem`.
```ini
[curl]
curl.cainfo="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
[openssl]
openssl.cafile="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
openssl.capath="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
```


_Tip: You can use git to store your configurations_