## ログインタイプ
```bash
2 # ユーザーがキーボードでログイン
3 # ネットワーク経由でフォルダ共有などにアクセス
5 # バックグラウンドのサービスが実行権限を得るためにアクセス
```

## イベントID
```bash
4624 # ログイン成功
```

## プロセス
```bash
secedit.exe /export # Windowsのセキュリティポリシーを書きだして検査する。
sdbinst.exe # Application Compatibility Database Installer。 古いソフトを動かすための互換性設定をインストールするツール。
```
