# Docker

## Centos安装Docker

### 基础条件

+ 系统为centos 7，要求系统为64位，且内核版本高于3.10

+ 系统为centos 6.5，要求系统为64为，且内核版本高于2.6.32-431

可执行`uname-r`查看系统内核信息

### docker安装

#### yum安装

1、安装必要的系统工具

`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`

2、添加软件源信息

`sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

3、更新yum缓存

`sudo yum makecache fast`

4、安装docker-ce

`sudo yum -y install docker-ce`

5、启动docker后台服务

`sudo systemctl start docker`


#### 脚本安装

1、确保yum更新到最新

`sudo yum update`

2、执行docker安装脚本

`curl -fsSL https://get.docker.com -o get-docker.sh`

`sudo sh get-docker.sh`

3、启动docker

`sudo systemctl start docker`


## MacOS安装Docker
