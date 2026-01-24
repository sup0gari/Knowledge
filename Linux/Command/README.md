# Discovery
```bash
id
hostname -I
cat /etc/passwd
pwd
ls # ファイル一覧を表示
ls -la # ファイル一覧を権限も含めて表示
cat # ファイルの中身を表示
strings # ファイルの中身の文字列のみ表示
stat # ファイル統計を表示
ss -tlunp # ポート情報の表示
crontab -l
cat /etc/crontab
ps aux # プロセス情報
sudo -l
grep -R -e "password" /var/log
checksec --file=file_name
readelf -l orvflw | grep STACK # GNU_STACKにEがあればNX無効
```

# shell
シェル関連のコマンド。
```bash
sudo su - # root
su - root # root
sudo -i # root
su - <User> # ユーザーの切り替え
sudo -u <User> <Command> # 指定したユーザーとしてsudoでコマンドを実行
```

# shell stabilization
## python3によるシェル安定化
```
python3 -c "import pty;pty.spawn('/bin/sh')"
```
## python3が使えない環境
```
script /dev/null -c bash
CTRL + Z
stty raw -echo
fg
export TERM=xterm
```

# Local name solution
ブラウザなどでドメインの名前解決の設定をするコマンド。
```
echo "<IP> <Domain>" | sudo tee -a /etc/hosts
```

# File download
```
wget <URL>
curl <URL> -o <Path>
```

# find
```bash
find <検索場所のパス> <オプション> <検索ファイル名>
-group <グループ名>
-type <ファイル形式> # ファイルならf, ディレクトリならd
-name <検索したいワード>
-iname <検索したいワード> # 大文字小文字を区別しない
-perm -4000 # SUID
-perm -2000 # SGID
-user <ユーザー名>
-exec <コマンド> # 検索内容に対してコマンド実行した結果を表示
```

# SUID, SGID file search
```bash
find / -perm -4000 -type f -exec ls -l {} \; 2>/dev/null # SUID
find / -perm -2000 -type f -exec ls -l {} \; 2>/dev/null # SGID
```
# EOF file creation
nanoやviなどのテキストエディタが使用できないときに使用。
## 変数を実行
`$NAME` => `sup0gari`
```bash
cat << EOF > hello.txt
Hello, $NAME.
EOF
```
## 変数を文字列とし、実行させない
`$NAME` => `$NAME`のまま
```bash
cat << 'EOF' > hello.txt
Hello, $NAME.
EOF
```

# Edit environment variables
```bash
export PATH=<YOUR PATH>
```

# Temp folder creation
```bash
mktemp -d # /tmp配下にディレクトリを作成する
cd $(mktemp -d)
```

# echo
```bash
-e # 特殊文字を解釈する
```

# grep
```bash
grep -E 'linux|windows' # linux, windowsどちらかを含む行を抽出
grep -v 'linux' # linuxを含む行以外を抽出
```

# sed
```bash
sed '3q' access.log # 3行目まで表示して終了
sed '1d' access.log # 1行目のみ表示
sed '/TEST/!d' access.log # TESTを含む行以外を非表示
sed '/TEST/d' access.log # TESTを含む行を非表示
sed -e 's/\[//g' -e 's/\]//g' access.log # -eで連結し、[と]を非表示にする。
sed '8,11s/^/    /' access.log # 8行目から11行目にマッチ
sed -n '1,3p' access.log # 1~3行目を表示
sed -n '/\/login\.php/p' access.log　# マッチした行のみ表示
```

# pcregrep
```bash
pcregrep -o '0x[0-9a-fA-F]+' access.log # 一致した箇所のみ抽出
pcregrep -o1 'PID:(\d+)' access.log # 一致した中で一つ目のカッコ内のみ抽出
pcregrep -v 'SUCCESS' access.log # 一致したもの以外の行を抽出
(?s) # .に改行を含ませる
.*?ABC # 次に出てきたABCまで一致
-i # 大文字小文字を区別しない
-M # 複数行に渡って一致させる
-A 1 # マッチした後の一行を表示
```

# nc file transfer
```bash
# 受信側
nc -lp 1234 > <File>
# 送信側
nc <Receiver IP> 1234 < <File>
```

# base64 file transfer
```bash
# 送信側
base64 <File> # 出力された文字列をコピー => base64 encoded string
# 受信側
base64 -d <base64 encoded string> > <File>
```

# obfuscation
```bash
${IFS} # スペースの代わり
$(<Command>) # コマンドインジェクションなどで使用
echo <base64 encoded string>|base64 -d|bash
echo -n <Payload> | base64 -d
```

# convert to hex
```
xxd -r -p <Hex file> > <ASCII File>
```

# zip file
```
unzip <File>
unzip <File> -d <Directory>
7z x <File>
```

# mdb
Microsoft Access Databaseという古いバージョンのデータベース形式
```
mdb-tables <File> # テーブルを表示
mdb-schema <File>
mdb-export <File> <Table>
mdb-sql <File> # SQLクエリを使用可能にする
mdb-sql -d '|' -P <File> # SQLクエリで見やすくパイプ区切りにする
```

# pst
Outlookで送受信したメールや、連絡先などのデータをローカルで保存するためのファイル形式。
```
readpst <File>
```

# tmux
1つのターミナル画面を複数のウィンドウに分割できるツール。  
ソケットファイルが公開されていて、読み書き権限があれば権限昇格できる。
```bash
ps aux
root       1027  0.0  0.1  26416  1676 ?        Ss   04:55   0:00 /usr/bin/tmux -S /.devs/dev_sess
tmux -S /.devs/dev_sess # rootターミナル起動
```

# keepass
ローカル管理型のオープンソース・パスワードマネージャー。`.kdbx`で保存される。
```bash
keepass2 <.kdbx>
```

# dig
DNSサーバーにドメイン名に関する情報を問い合わせるためのコマンド。
```bash
dig @<Target> <Domain> AXFR # 成功すると内部ネットワーク構成情報がわかる
```

# nslookup
ドメイン名とIPアドレスを変換するコマンド
```bash
nslookup <Target> <DNS>
```
