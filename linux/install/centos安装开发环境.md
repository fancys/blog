# Centos安装开发环境

## java环境安装

### jdk安装
参考[Centos安装jdk](./Centos安装jdk.md)

### git安装

进入下载目录
`cd /soft`

下载新版本git
`wget https://www.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz`

解压
`tar -xzvf git-2.9.5.tar.gz -C /usr/local`

安装
`cd /usr/local/git-2.9.5`

编译git
编译前安装
`curl`
`yum install -y curl-devel`

1、`make configure`

2、`./configure --prefix=/usr/local/git`

3、`make prefix=/usr/local/git`
**编译过程如果报错，一般是缺少依赖包，使用yum安装相应包即可**

配置环境变量
`echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/profile`

让配置文件生效
`source /etc/profile`

检查版本
`git --version`

### maven安装

下载maven包
`wget http://mirrors.hust.edu.cn/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz`

解压并移到usr/local
`tar -xzvf apache-maven-3.6.2-bin.tar.gz -C /usr/local`

重命名
`mv apache-maven-3.6.2-bin.tar.gz maven3`

配置环境变量
`echo "export PATH=$PATH:/usr/local/maven3/bin" >> /etc/profile`

## 前端环境安装


