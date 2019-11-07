# Centos安装jdk

## 安装包安装

## yum工具安装

### 1、搜索jdk
`yum search java|grep jdk`

### 2、下载jdk
`yum install java-1.8.0-openjdk`

### 3、配置环境变量
`vi /etc/profile`

```shell
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_181
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
```
### 4、验证是否配置正确
`java -version`



