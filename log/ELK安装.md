## ELK安装

### 概念
    `ELK`是`Elasticsearch`,`Logstash`,`Kibana`三个组件的首字母缩写，通常组合在一起实现分布式日志消息系统。
    + `Elasticsearch` 基于Json的分布式搜索引擎
    + `Logstash` 日志采集工具
    + `Kibana` 消息面板，展示日志消息

### 通过安装包安装





## 通过Docker安装

+ `Elasticsearch`安装

1. 创建本地映射文件夹
```shell
 mkdir data/docker-data/elasticsearch/config
```
2. 拉取`Elasticsearch`镜像
```shell
 docker pull elasticsearch:6.7.8
```
3. 创建容器，并复制配置文件
```shell
docker run -d --name es --net elk  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  elasticsearch:6.8.9
docker cp /usr/share/elasticsearch/config/ /data/docker-data/elasticsearch

```
4. 重新创建容器
```shell
docker stop es
docker rm es
docker run -d --name es --net elk  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" \
-v /data/docker-data/elasticsearch/config/:/usr/share/elasticsearch/config/ --privileged=true elasticsearch:6.8.9
```

5. 校验是否成功
浏览器访问`http://localhost:9200`


+ `Logstash`安装

1. 创建容器，并赋值配置文件
```shell
docker run --name logstash -d -p 5044:5044 --net esnet logstash:6.8.9
docker cp logstash:/usr/share/logstash/config /data/docker-data/logstash/
docker cp logstash:/usr/share/logstash/data /data/docker-data/logstash/
docker cp logstash:/usr/share/logstash/pipeline /data/docker-data/logstash/
```

2. 重新创建容器
```shell
docker stop logstash
docker rm logstash
docker run --name logstash -d --net elk -p 5044:5044 \
-v /data/docker-data/logstash/config:/usr/share/logstash/config \
-v /data/docker-data/logstash/pipeline/:/usr/share/logstash/pipeline/ \
logstash:6.8.9
```

+ `Kibana`安装

1. 创建本地映射文件
```shell
 mkdir /data/docker-data/kibana/config
 cd config
 vim kibana.yml
```
`kibana.yml`内容：
```yml
server.name: kibana
server.host: "0"
elasticsearch.hosts: [ "http://ip:9200" ]
xpack.monitoring.ui.container.elasticsearch.enabled: true
```
2. 创建容器
```shell
docker run -d --name kibana --net elk -v /data/docker-data/kibana/config:/usr/share/kibana/config -p 5601:5601 kibana:6.8.9
```

3. 校验是否成功启动

浏览器访问`http://localhost:5601`