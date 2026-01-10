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
