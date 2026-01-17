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
sed -e 's/\[//g' -e 's/\]//g' access.log # -eで連結し、[と]を非表示にする。
```

## pcregrep
```bash
pcregrep -o '0x[0-9a-fA-F]+' access.log # 一致した箇所のみ抽出
pcregrep -o1 'PID:(\d+)' access.log # 一致した中で一つ目のカッコ内のみ抽出
pcregrep -v 'SUCCESS' access.log # 一致したもの以外の行を抽出
(?s) # .に改行を含ませる
.*?ABC # 次に出てきたABCまで一致
```
