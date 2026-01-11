## プロセスの起動
Powershellからcalc.exeの例  
1. ユーザー入力  
`powershell.exe`の入力文字列をパースし、環境変数`PATH`から`calc.exe`を見つける。`System.Management.Automation.dll`が動く。
2. ユーザーモード（Win32 API）  
`kernel32.dll`の`CreateProcessW`が呼ばれる。
3. ユーザーモード（Native API）
`ntdll.dll`の`NtCreateUserProcess`または`ZwCreateUserProcess`が呼ばれる。
4. カーネルモード（システムコール）
`syscall`の実行で`ntoskrnl.exe`に実行権限を渡す。カーネルはメモリ空間を確保し、`calc.exe`の実行バイナリを展開。
5. プロセスの誕生  
`ntdll.dll`の`LdrLoadDll`が呼ばれる。ここでプロセス生成ログがつくられる。
6. プロセスの通知  
カーネルの`PsSetCreateProcessNotifyRoutine`という機能により通知される。

## exe
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
mofcomp.exe # .mofファイルを読み取り、WMIリポジトリが理解できるようにコンパイルする
```
