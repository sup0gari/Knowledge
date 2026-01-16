## 複数条件のgrep
`grep -E 'test|test2'`

## echoのオプション
```bash
echo -e # 特殊文字を解釈する
```

## sed
```bash
sed '3q' access.log # 3行目まで表示して終了
sed '1d' access.log # 1行目のみ表示
sed '/TEST/!d' access.log # TESTを含む行以外を非表示
sed '/TEST/d' access.log # TESTを含む行を非表示
```
