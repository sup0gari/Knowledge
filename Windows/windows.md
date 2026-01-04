## Windows起動時の挙動
1. カーネルロード(UEFI/BIOS)  
マザーボードがハードウェアをチェックし、カーネル(`ntoskrnl.exe`)をメモリに読み込む。
2. ELAM(Early Launch Anti-Malware)の起動  
カーネルの直後にセキュリティソフトのドライバを他のサードパーティー製ドライバよりも先に読み込む。
3. システムサービスの起動  
Session Manager(`smss.exe`)が起動し、`lsass.exe`, `services.exe`, `wininit.exe`もこのタイミングで起動する。
4. ユーザーログインとシェル  
`winlogon.exe`がログイン画面やCtrl + Alt + Delを表示する。パスワードが通ると`userinit.exe`が走り、`explorer.exe`が起動。

## ユーザーモード(User Mode/Ring3)
基本的なエンドユーザーが使用しているアプリが動く制限されたエリア。ハードウェア（メモリやディスク）に直接触れず、何かする時は`.dll`の中のWin32 APIを通じてOSに命令する。
クラッシュした際はOSに影響を及ぼすことはなく、アプリが落ちるだけで済む。

## カーネルモード(Kernel Mode/Ring0)
OSの中枢が動く無制限のエリア。カーネル(`ntoskrnl.exe`)やデバイスドライバが動く。CPUやメモリのすべてのリソースにアクセスできる。クラッシュした際はブルースクリーン(BSOD)になり、システム全体が強制終了する。

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

## WindowsEventID
```bash
4624 # ログイン成功
4625 # ログイン失敗
4648 # 明示的な資格情報を使用したログイン
4672 # 特権の割り当て、管理者権限でのログイン
4720 # ユーザーアカウントの作成
4688 # 新しいプロセスの作成
```

## SysmonID
```bash
1 # プロセスの作成
7 # モジュール、DLLのロード
10 # プロセスへのアクセス
8 # リモートスレッドの作成、あるプロセスが別のメモリでコードを動かす動き。
3 # ネットワーク接続
6 # ドライバのロード
11 # ファイルの作成
12, 13, 14 # レジストリの操作
```

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
```

## Windows Defenderの停止
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
Set-MpPreference -MAPSReporting Disabled
Set-MpPreference -SubmitSamplesConsent NeverSend
```

## リネームセクション
Windowsの実行ファイルにはリネームセクションという情報があり、ファイル名を変更しても元のファイル名がわかる。  

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
