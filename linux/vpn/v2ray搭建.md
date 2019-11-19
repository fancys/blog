# v2ray搭建

### 环境
+ vps Vultr
+ centos 8



### 1、下载shell脚本
```shell
    wget https://install.direct/go.sh
```
如果未安装`wget`命令，先安装`yum install wget`;

### 2、安装unzip

```shell
yum install -y zip unzip
```

### 3、执行安装

***注意：最好使用root用户安装***
```shell
 bash go.sh
```

### 4、相关命令

```shell
## 启动
systemctl start v2ray

## 停止
systemctl stop v2ray

## 重启
systemctl restart v2ray

## 开机自启
systemctl enable v2ray
```

### 查看配置
```shell
    cat /etc/v2ray/config.json
```

注意几个主要信息：端口、id、alterId,这些信息要与客户端保持一致。

### 打开防火墙


### 客户端配置

[v2ray-windows-64.zip Github Release](https://github.com/v2ray/v2ray-core/releases)
[v2rayN-v2rayN.exe-Github Release](https://github.com/2dust/v2rayN/releases)

对v2ray-windows-64.zip进行解压，然后将下载的V2RayN.exe复制到解压后的目录，即两个下载好的文件需要在同一目录。
启动客户端完成配置后，选择PAC代理模式。


### 参考
[V2Ray完全使用教程](https://zshttp.com/1310.html)


