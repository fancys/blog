

### Etcd集群搭建

#### 一、静态搭建

#### 1. 机器配置

| 机器名称      | 配置     | 端口       |
| ------------- | -------- | ---------- |
| Etcd-Master   | 1G/1Core | 2379，2380 |
| Etcd-Slave-01 | 1G/1Core | 2379，2380 |
| Etcd-Slave-02 | 1G/1Core | 2379，2380 |

端口设置：

```
firewall-cmd --zone=public --add-port=2379/tcp --permanent
firewall-cmd --zone=public --add-port=2380/tcp --permanent
firewall-cmd --reload
```

#### 2. 安装配置

1. 安装etcd

   ```shell
   yum install -y etcd
   ```

   校验是否安装成功：

   ```shell
   [root@etcd-master etcd]# rpm -qa etcd
   etcd-3.3.11-2.el7.centos.x86_64
   ```

   

2. 配置文件修改

   1） 先备份一个原有配置文件

   ```
   cp/etc/etcd/etcd.conf /etc/etcd/etcd.conf.bak
   ```

   2）修改etcd.conf配置文件

   将http://localhost都改成对应机器ip：

   ```shell
   #[Member]
   #ETCD_CORS=""
   ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
   #ETCD_WAL_DIR=""
   ETCD_LISTEN_PEER_URLS="http://192.168.64.68:2380"
   ETCD_LISTEN_CLIENT_URLS="http://192.168.64.66:2379"
   #ETCD_MAX_SNAPSHOTS="5"
   #ETCD_MAX_WALS="5"
   ETCD_NAME="Master"
   #ETCD_SNAPSHOT_COUNT="100000"
   #ETCD_HEARTBEAT_INTERVAL="100"
   #ETCD_ELECTION_TIMEOUT="1000"
   #ETCD_QUOTA_BACKEND_BYTES="0"
   #ETCD_MAX_REQUEST_BYTES="1572864"
   #ETCD_GRPC_KEEPALIVE_MIN_TIME="5s"
   #ETCD_GRPC_KEEPALIVE_INTERVAL="2h0m0s"
   #ETCD_GRPC_KEEPALIVE_TIMEOUT="20s"
   #
   #[Clustering]
   ETCD_INITIAL_ADVERTISE_PEER_URLS="http://192.168.64.68:2380"
   ETCD_ADVERTISE_CLIENT_URLS="http://192.168.64.68:2379"
   #ETCD_DISCOVERY=""
   #ETCD_DISCOVERY_FALLBACK="proxy"
   #ETCD_DISCOVERY_PROXY=""
   #ETCD_DISCOVERY_SRV=""
   ETCD_INITIAL_CLUSTER="Master=http://192.168.64.66:2380,Slave01=http://192.168.64.67:2380,Slave02=http://192.168.64.68:2380"
   ETCD_INITIAL_CLUSTER_TOKEN="etcd-cluster"
   ETCD_INITIAL_CLUSTER_STATE="new"
   #ETCD_STRICT_RECONFIG_CHECK="true"
   #ETCD_ENABLE_V2="true"
   #
   #[Proxy]
   #ETCD_PROXY="off"
   #ETCD_PROXY_FAILURE_WAIT="5000"
   #ETCD_PROXY_REFRESH_INTERVAL="30000"
   #ETCD_PROXY_DIAL_TIMEOUT="1000"
   #ETCD_PROXY_WRITE_TIMEOUT="5000"
   #ETCD_PROXY_READ_TIMEOUT="0"
   
   ```

   ***注意ETCD_NAME的值需要与ETCD_INITIAL_CLUSTER中的节点名称一致***

   启动etcd服务：

   ```
   systemctl start etcd
   ```

   验证集群是否搭建成功：

   ````
   [root@etcd-master etcd]# etcdctl member list
   [root@etcd-master etcd]# etcdctl ls
   Error:  client: etcd cluster is unavailable or misconfigured; error #0: dial tcp 127.0.0.1:2379: connect: connection refused
   ; error #1: dial tcp 127.0.0.1:4001: connect: connection refused
   
   error #0: dial tcp 127.0.0.1:2379: connect: connection refused
   error #1: dial tcp 127.0.0.1:4001: connect: connection refused
   ````

   发现报错，此处需要在ETCD_LISTEN_CLIENT_URLS将127.0.0.1:2379配置也加上：

   ````
   ETCD_LISTEN_PEER_URLS="http://192.168.64.68:2380"
   ETCD_LISTEN_CLIENT_URLS="http://192.168.64.66:2379,http://127.0.0.1:2379"
   ````

   然后重启etcd服务即可：

   ````
   systemctl restart etcd
   ````

   

   

