# PSReadLine
Powershellの標準機能であり、履歴がファイルに書き込まれる。  
`C:\Users\<User>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt`

# ETW
Event Tracing for Windows。OSやアプリケーションの挙動をリアルタイムに監視しているカーネルレベルのシステム。
## Provider
ファイルを開く、書き込むなどのイベントを発生させる。
## Controller
どのProviderからどの情報を取るか決める。
## Consumer
流れてきたイベントを受け取って処理をする。
# pst
Outlookで送受信したメールや、連絡先などのデータをローカルで保存するためのファイル形式。
# PHP cookie path
`windows/temp/sess_XXXXXXXXX`

# .sct
`.sct`とはXML形式のスクリプトレットファイルで、デフォルトだとWSHがサポートしているJScript, VBScriptの実行が可能。
```xml
<script language="JScript">
    // ここにJavaScriptを書く
</script>

<script language="VBScript">
    ' ここにVBScriptを書く
</script>
```

# 名前解決
`C:\Windows\System32\drivers\etc\hosts`を編集する。
```
192.168.56.111 example.local
```

# PPL
Protected Process Light。正当な署名を持つセキュリティプロセス以外は、アクセスを拒否するセキュリティ機能。
lsass.exeのダンプやmimikatz.exeを経由したダンプはこれにより拒否される可能性が高い。

# DSE
Driver Signature Enforcement。カーネルドライバーをロードする際に、そのドライバーがMicrosoftによってデジタル署名され、検証されているかを確認するセキュリティ機能。

# KB
Windowsのアップデート（パッチ）一つ一つに割りふられた管理番号。そのパッチが何の不具合を直し、何の脆弱性を修正したかを特定するためのID。

# リネームセクション
Windowsの実行ファイルにはリネームセクションという情報があり、ファイル名を変更しても元のファイル名がわかる。  

# Windows Prefetchとは
アプリケーションの起動を高速化するために、Windowsがプログラムの起動に必要な情報（どのファイルやメモリ領域を使うか）を事前に記録・キャッシュしておく仕組みと、その記録ファイルのこと

# MFTとは
NTFSファイルシステムで全てのファイルとフォルダのメタデータ（名前、サイズ、作成日時、ディスク上の場所など）を記録・管理する中心的なデータベース

# library-msとは
ローカルやネットワーク上にある複数のフォルダを仮想的に1つにまとめて管理するバーチャルフォルダへのショートカットや実体として機能する。
## CVE-2025-24071
ZIPファイルを展開した際に、中にある細工された`.library-ms`ファイル(xml)が自動的に読み込まれる。このファイル内の`<url>`タグにUNCパスを指定しておくとRessponderなどでNTLMハッシュを奪取できる。  
https://github.com/0x6rss/CVE-2025-24071_PoC/tree/main
