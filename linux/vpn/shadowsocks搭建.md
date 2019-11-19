# shadowsocks搭建

### 下载执行脚本
```shell
## 下载wget
yum install wget

## 下载shell脚本
wget –no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
```

### 修改权限

```shell
chmod +x shadowsocks.sh
```

### 执行脚本
```shell
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```
以下为我安装过程中遇到的错误及解决方案。

#### gcc环境
```shell
yum install gcc
```

#### libsodium环境
报错`libsodium install failed!`,缺少`libsodium`环境
```shell
 yum install epel-release -y
 yum install libsodium -y
```

#### make环境

```shell
 yum install make -y
```

#### python环境
`shadowsocks.sh`基于`python`编写，需要安装python环境

+ 如果系统安装的python3，需要将shell脚本中`python`命令改成`python3`

+ 同时将dist-packages文件所在目录，拷贝到/usr/local/lib/目录下
```shell
    cp -r /usr/lib/python3.6 /usr/local/lib/.
```

### 客户端

[shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows/releases)

完成配置后，启用系统代理，选择`PAC模式`。

### 使用BBR加速上网

```shell
##下载bbr
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

##修改权限
chmod +x bbr.sh

##启动bbr
./bbr.sh

##检查是否启动
lsmod | grep bbr
```


