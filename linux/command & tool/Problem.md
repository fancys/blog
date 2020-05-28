# Problem集合


## docker相关

执行`docker info`命令出现
```linux
 Got permission denied while trying to connect to the Docker daemon socket Got permission denied while trying to connect to the Docker daemon socket
```

*解决方案*：
将当前用户添加到`docker`用户组中
```linux
sudo gpasswd -a $USER docker
```


## shell相关

执行shell命令报错：`$'\r': command not found`
这是由于windows环境下编辑，换行符为`\r`,而linux下换行符为`\n`，无法识别。

*解决方案*
安装`dos2unix`工具，使用`dos2unix`进行转码
1、`yum install dos2unix`
2、`dos2unix shell脚本`
3、执行shell脚本

或

用`vi`或`vim`进入脚本，在命令模式下输入`set staff`,查看当前脚本环境。








