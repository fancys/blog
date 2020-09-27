### kafka安装



#### 1、docker安装

`1、镜像拉取`

```java
docker pull wurstmeister/zookeeper
docker pull wurstmeister/kafka
```

`2、创建网络`

```docker
docker network create -d bridge kafka-net
```

`3、启动容器`

```docker
docker run -d --name zookeeper --network kafka-net -p 2181:2181 -v /etc/localtime:/etc/localtime wurstmeister/zookeeper

docker run --name kafka \
--network kafka-net \
-p 9092:9092 \
-e KAFKA_BROKER_ID=0 \
-e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://127.0.0.1:9092 \
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 \
-v /Users/fanxing/docker/kf-zk/localtime:/etc/localtime \
 -d wurstmeister/kafka 
```

`4、验证kafka是否正常`

```shell
//1.进入kafka容器，运行生产者发送消息
# docker exec -it kafka /bin/sh
# cd /opt/kafka_2.13-2.6.0/bin
# ./kafka-console-producer.sh --broker-list localhost:9092 --topic kafka-test
# hello kafka

//2.另启一个窗口，运行kafka消费者接收消息
# kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic kafka-test --from-beginning

```

