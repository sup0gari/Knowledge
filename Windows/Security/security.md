## PPL（Protected Process Light）とは
正当な署名を持つセキュリティプロセス以外は、アクセスを拒否するセキュリティ機能。  
`lsass.exe`のダンプや`mimikatz.exe`を経由したダンプはこれにより拒否される可能性が高い。

## DSE（Driver Signature Enforcement）とは
カーネルドライバーをロードする際に、そのドライバーがMicrosoftによってデジタル署名され、検証されているかを確認するセキュリティ機能。  
この機能はBYOVDによってバイパスされる可能性があるため、注意が必要。

## KB（Knowledge Base）とは
Windowsのアップデート（パッチ）一つ一つに割りふられた管理番号。そのパッチが何の不具合を直し、何の脆弱性を塞いだかを特定するためのID。
