## プロセス
```bash
ntoskrnl.exe # Windowsの核（カーネル）。ハードウェアとソフトの仲介。
smss.exe # セッションマネージャー。最初に動くユーザーモードプロセス。
lsass.exe # ローカルセキュリティ権限。ログイン認証とパスワード管理。
services.exe # サービスコントロールマネージャー。バックグラウンドサービスを管理。
wininit.exe # Windowsの初期化。Session 0（システム用）の管理。
winlogon.exe # ログオンアプリケーション。Ctrl+Alt+Delの処理や画面ロック。
userinit.exe # ユーザー環境の初期化。ログイン直後に一度だけ動く。
explorer.exe # Windowsシェル。デスクトップやタスクバー、フォルダ表示。
csrss.exe # プロセスやスレッドの作成、コンソールウィンドウの管理。
svchost.exe # services.exeから起動されるDLL形式のサービスを実行する。
conhost.exe # コンソールの出力管理。
secedit.exe /export # Windowsのセキュリティポリシーを書きだして検査する。
sdbinst.exe # Application Compatibility Database Installer。 古いソフトを動かすための互換性設定をインストールするツール。
pubprn.vbs # プリンターをActive Directoryに発行（登録）するために使用されるMicrosoft提供のVBScript
manage-bde.wsf # WindowsのBitLockerドライブ暗号化をコマンドラインから管理するためのVBScriptファイル
```
