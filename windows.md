## Windows起動時の挙動
1. カーネルロード(UEFI/BIOS)  
マザーボードがハードウェアをチェックし、カーネル(`ntoskrnl.exe`)をメモリに読み込む。
2. ELAM(Early Launch Anti-Malware)の起動  
カーネルの直後にセキュリティソフトのドライバを他のサードパーティー製ドライバよりも先に読み込む。
3. システムサービスの起動  
Session Manager(`smss.exe`)が起動し、`lsass.exe`, `services.exe`, `wininit.exe`もこのタイミングで起動する。
4. ユーザーログインとシェル  
`winlogon.exe`がログイン画面やCtrl + Alt + Delを表示する。パスワードが通ると`userinit.exe`が走り、`explorer.exe`が起動。

## ログインタイプ
```bash
2 # ユーザーがキーボードでログイン
3 # ネットワーク経由でフォルダ共有などにアクセス
4 # タスクスケジューラなどで実行
5 # バックグラウンドのサービスが実行権限を得るためにアクセス
7 # 画面ロックの解除
9 # runas /netonlyなどの特殊なログオン
10 # RDP
```

## イベントID
```bash
4624 # ログイン成功
4625 # ログイン失敗
4648 # 明示的な資格情報を使用したログイン
4672 # 特権の割り当て、管理者権限でのログイン
4720 # ユーザーアカウントの作成
4688 # 新しいプロセスの作成
```

## プロセス
```bash
ntoskrnl.exe # Windowsの「核（カーネル）」。ハードウェアとソフトの仲介。
smss.exe # セッションマネージャー。最初に動くユーザーモードプロセス。
lsass.exe # ローカルセキュリティ権限。ログイン認証とパスワード管理。
services.exe # サービスコントロールマネージャー。バックグラウンドサービスを管理。
wininit.exe # Windowsの初期化。Session 0（システム用）の管理。
winlogon.exe # ログオンアプリケーション。Ctrl+Alt+Delの処理や画面ロック。
userinit.exe # ユーザー環境の初期化。ログイン直後に一度だけ動く。
explorer.exe # Windowsシェル。デスクトップやタスクバー、フォルダ表示。
csrss.exe # プロセスやスレッドの作成、コンソールウィンドウの管理。
svchost.exe # services.exeから起動されるDLL形式のサービスを実行する。
secedit.exe /export # Windowsのセキュリティポリシーを書きだして検査する。
sdbinst.exe # Application Compatibility Database Installer。 古いソフトを動かすための互換性設定をインストールするツール。
```
