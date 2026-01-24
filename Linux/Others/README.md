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
