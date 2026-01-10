## PPL（Protected Process Light）とは
正当な署名を持つセキュリティプロセス以外は、アクセスを拒否するセキュリティ機能。  
`lsass.exe`のダンプや`mimikatz.exe`を経由したダンプはこれにより拒否される可能性が高い。

## DSE（Driver Signature Enforcement）とは
カーネルドライバーをロードする際に、そのドライバーがMicrosoftによってデジタル署名され、検証されているかを確認するセキュリティ機能。  
この機能はBYOVDによってバイパスされる可能性があるため、注意が必要。
