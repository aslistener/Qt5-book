# 安装sysroot

build/linux/sysroot_scripts/sysroot-creator-sid.sh
下载package的url，以及各个平台所需要的库

build/linux/sysroot_scripts/sysroot-creator.sh
拼接下载url，然后从 https://snapshot.debian.org/archive/debian 下载，然后放在

build/linux/sysroot_scripts/sysroots.json
各个平台安装包对应的sha1值，install sysroot时会使用到这个值

build/linux/sysroot_scripts/install-sysroot.py
使用 sysroot.json里面的sha1值，把sysroot 打包上传到
https://commondatastorage.googleapis.com/.../(sha1)目录下面




# 交叉编译
