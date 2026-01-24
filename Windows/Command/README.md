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
