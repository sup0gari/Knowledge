# SSH
ssh関連のファイルパス、ファイル名など。
```bash
~/.ssh/
~/.ssh/authorized_keys # server
~/.ssh/known_hosts # client
id_rsa
id_ed25519
id_rsa.pub
id_ed25519.pub
```

# id
## EUID
実行権限を指す。0がroot。
## RUID
ログインユーザーのID。
## SUID
有効な場合、実行時に所有者の権限で実行する。
## SGID
有効な場合、実行時に所有グループの権限で実行する。

# sudoers
一般ユーザーが sudo コマンドを使用する際に、どのコマンドを、どのユーザーの権限で、パスワードなしで実行できるかを定義したファイル。  
`sudo -l`で見ることができる。`/etc/sudoers`が通常のファイルパス。
