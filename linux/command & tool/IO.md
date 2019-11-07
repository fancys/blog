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

`cp 源文件路径 目标目录`


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








