# Command
## shell
シェル関連のコマンド。
```bash
sudo su - # root
su - root # root
sudo -i # root
su - <User> # switch <User>
sudo -u <User> <Command> # execute the command as a <User> 
```
## shell stabilization
### python3によるシェル安定化
```
python3 -c "import pty;pty.spawn('/bin/sh')"
```
### python3が使えない環境
```
script /dev/null -c bash
CTRL + Z
stty raw -echo
fg
export TERM=xterm
```
## Local name solution
ブラウザなどでドメインの名前解決の設定をするコマンド。
```
echo "<IP> <Domain>" | sudo tee -a /etc/hosts
```
## File download
```
wget <URL>
curl <URL> -o <Path>
```
## SUID, SGID file search
```bash
find / -perm -4000 -type f -exec ls -l {} \; 2>/dev/null # SUID
find / -perm -2000 -type f -exec ls -l {} \; 2>/dev/null # SGID
```
## EOF file creation
nanoやviなどのテキストエディタが使用できないときに使用。
### 変数を実行
`$NAME` => `sup0gari`
```bash
cat << EOF > hello.txt
Hello, $NAME.
EOF
```
### 変数を文字列とし、実行させない
`$NAME` => `$NAME`のまま
```bash
cat << 'EOF' > hello.txt
Hello, $NAME.
EOF
```
## Edit environment variables
```bash
export PATH=<YOUR PATH>
```
## Temp folder creation
```
mktemp -d
cd $(mktemp -d)
```
## sed
## pcregrep
## nc file transfer
## base64 file transfer
# Assembly
# Others
## SSH
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
## Group
### adm
システムのログファイル（`/var/log`）の読み取り権限。
### lxd
