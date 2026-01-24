# Command
## shell stabilization
### python3によるシェル安定化
```
python3 -c "import pty;pty.spawn('/bin/sh')"
```
### python3が使えない環境
```
script /dev/null -c bash
CTRL + Z
stty raw -echo
fg
export TERM=xterm
```
# Assembly
# Others
