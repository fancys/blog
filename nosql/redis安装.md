## redis安装



### 源代码编译

1. 下载源代码

   进入[redis官网](https://redis.io/)，找到Download地址

   ```shell
   wget http://download.redis.io/releases/redis-5.0.8.tar.gz
   ```

2. 编译源代码

   编译redis代码，需先安装`gcc`

   ```shell
   yum install gcc
   ```

   然后执行`make`命令

   ```shell
   make
   ```

3. 编译安装包

   编译后我们可以进入src目录，执行`redis_server`脚本，但是我们一边会安装redis

   ```shell
   # PREFIX为redis的安装目录
   make install PREFIX=/opt/redis
   ```

4. 设置环境变量

   ```shell
   vim /etc/profile
   ```

   追加两行：

   ```shell
   export REDIS_HOME=/opt/redis
   export PATH=$PATH:$PATH/bin
   ```

   让配置文件立即生效：

   ```shell
   source /etc/profile
   ```

   查看配置文件是否正确

   ```shell
   echo $PATH
   ```

5. 执行安装

   进入utils目录，执行instal_redisl.sh脚本

   ```shell
   cd utils
   ./install_redis.sh
   ```

   安装成功后，默认redis是启动的。
   
   `cd /etc/init.d ` 查看自动启动的服务，默认是`redis_6379`
   
   `service redis_6379 status`查看服务状态，或者`ps -fe | grep redis`查看redis相关启动的进程。
   
   
   
6. 进入客户端

   `redis-cli` 会直接进入默认本机默认端口（6379）的redis进程。

### docker安装

