# Linxu 下关于IO操作命令

## 文件夹

+ 新建`document`文件夹

    `mkdir document`

+ 删除空目录document

    `rmdir document`

+ 删除文件夹

    `rm -rf 文件夹名称`
    `-r` 表示向下递归，
    `-f` 表示强删，不需询问确认

+ 重命名与剪切

剪切文件夹 `mv 文件名 移动目的地文件名`
重命名     `mv 文件名 修改后的文件名`

## 文件

+ 查找当前目录下，文件名以ava开头的文件
`find . -name ava\*`

+ 查看文件profile的内容
`cat profile`

+ 修改文件日期
`touch filename`
如果文件不存在，则新建文件

### 拷贝

命令：`cp [option] src dest`

option参数说明：
```shell
-a:是指archive的意思，也说是指复制所有的目录

-d:若源文件为连接文件(link file)，则复制连接文件属性而非文件本身

-f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制

-i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作

-l:建立硬连接(hard link)的连接文件，而非复制文件本身

-p:与文件的属性一起复制，而非使用默认属性

-r:递归复制，用于目录的复制操作

-s:复制成符号连接文件(symbolic link)，即“快捷方式”文件

-u:若目标文件比源文件旧，更新目标文件
```


1. 文件拷贝

    `cp 源文件路径 目标目录`

2. 文件夹拷贝

    `cp -r /data /temp/`  将`data`目录拷贝到`temp`目录下，形成`/temp/data/**`

3. 强制替换、覆盖文件

    `\cp -r /data /temp` 将/data目录下的文件复制到/temp目录下，有相同名称的强制替换。



### 压缩与解压

常见的压缩格式
+ zip
+ gz
+ bz2
+ tar.gz
+ tar.bz2

如使用压缩命令未找到，需先安装压缩命令
如安装zip命令：`yum -y install zip`

*zip压缩解压*
+ 压缩文件
`zip 压缩文件名 源文件`
+ 压缩目录
`zip -r 压缩文件名 源目录`
+ 解压
`unzip 压缩文件名`








