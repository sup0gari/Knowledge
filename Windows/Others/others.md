## Windows Defenderの停止
```powershell
Set-MpPreference -DisableBehaviorMonitoring $true
Set-MpPreference -DisableIntrusionPreventionSystem $true
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableBlockAtFirstSeen $true
```

## .sctとは
`.sct`とはXML形式のスクリプトレットファイルで、デフォルトだとWSHがサポートしているJScript, VBScriptの実行が可能。
```xml
<script language="JScript">
    // ここにJavaScriptを書く
</script>

<script language="VBScript">
    ' ここにVBScriptを書く
</script>
```

## 名前解決
`C:\Windows\System32\drivers\etc\hosts`を編集する。
```
192.168.56.111 example.local
```

## 特定フォルダのセキュリティ除外
`Add-MpPreference -ExclusionPath "C:\tools"`

## ロード中のDLLリストの取得
```powershell
$modules = [System.Diagnostics.Process]::GetCurrentProcess().Modules
```

## FW確認
```powershell
# FWルール表示
netsh advfirewall firewall show rule name=all
# ホストオンリーアダプターに対してFWを無効にする
netsh advfirewall firewall add rule name="hostonlyadaptor" dir=in action=allow remoteip=192.168.56.0/24
```

## ポート状態表示
```powershell
netstat -abno
```
