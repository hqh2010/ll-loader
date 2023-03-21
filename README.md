# Linglong Loader

# 说明

本项目是演示通过squashfs（也可以通过erofs）将一个可执行程序打包成另外一个自定义格式的可执行文件．其中loader是原目标文件，appid_version_arch.uap是打包后的自定义可执行目标文件．

## Getting started

```bash
sudo apt install squashfuse fuse -y
mkdir build && cd build 
cmake .. && make -j1

# 剥离调试符号（可以不做） 
strip bin/linglong-loader
# 制作 data.sqsfs
mkdir work
cd work
touch loader
# loader中输入以下内容，loader模拟直正的应用中的二进制

#!/bin/bash
echo "This is a Default Loader"
echo $@

# 给loader增加执行权限
chmod +x loader
cd ..
mksquashfs work/loader work/* data.sqsfs -comp xz
cat bin/linglong-loader data.sqsfs > appid_version_arch.uap
chmod +x appid_version_arch.uap
```
