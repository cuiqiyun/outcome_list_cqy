# 使用RuyiSDK 编译 Milk-V Duo256m 示例程序教程

本教程以在Milk-v Duo256m上运行2048游戏为例，验证RuyiSDK的终端 IO、ANSI 转义码、单文件编译等功能



## 1.准备工作

### 下载系统镜像

```
https://github.com/milkv-duo/duo-buildroot-sdk-v2/releases/download/v2.0.1/milkv-duo256m-musl-riscv64-sd_v2.0.1.img.zip
```



### 解压系统镜像

```
unzip milkv-duo256m-musl-riscv64-sd_v2.0.1.img.zip
```



### 系统镜像烧录进存储卡

```
sudo dd if=milkv-duo256m-musl-riscv64-sd_v2.0.1.img of=/dev/sdX bs=1M; sync
```



### 安装依赖包

```
sudo apt update; sudo apt install -y wget tar zstd xz-utils git build-essential
```



### 安装ruyi包管理器

```
wget https://mirror.iscas.ac.cn/ruyisdk/ruyi/tags/0.45.0/ruyi-0.45.0.amd64
chmod +x ruyi-0.45.0.amd64
sudo cp -v ruyi-0.45.0.amd64 /usr/local/bin/ruyi
```



### 安装GCC工具链

```
ruyi update
ruyi install gnu-plct 
```



## 2.在GCC虚拟环境中编译2048.c

### 创建并激活ruyi虚拟环境（GCC)

```
ruyi venv -t toolchain/gnu-plct milkv-duo venv-2048
. ~/venv-2048/bin/ruyi-activate
```



### 验证GCC版本

```
riscv64-plct-linux-gnu-gcc -v
```



### 获取2048的源码

```
wget https://raw.githubusercontent.com/mevdschee/2048.c/master/2048.c
```



### 编译2048.c

```
riscv64-plct-linux-gnu-gcc -std=c99 -Wextra -march=rv64gc -mabi=lp64d -o 2048 2048.c -lc
```



### 将GCC构建的二进制传输至开发板

```
scp ../2048 root@192.168.42.211:~
```



 ### 赋予执行权限

```
chmod +x 2048
```

 

### 运行2048

```
./2048
```

