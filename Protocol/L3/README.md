# L3とは

## その他
```powershell
# kali(vm)からwindows11(host)への通信をFWで許可する設定コマンド(ホストオンリーアダプター)
netsh advfirewall firewall add rule name="hostonlyadaptor" dir=in action=allow remoteip=192.168.56.0/24
```
