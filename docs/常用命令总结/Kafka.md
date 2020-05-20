新版本kafka 0.8.0之后建议使用， **--bootstrap-server** 以下是0.8.0之前的命令用法，如需使用新版本命令只需要将--zookeeper替换为--bootstrap-server即可

- 启动kafka服务

```
bin/kafka-server-start.sh config/server.properties
```

- 创建topic(指定多个ip:port逗号分隔)

    - replication-factor个数不能超过broker的个数
    - partitions分区数
    - replication每个分区拥有的副本数

```
kafka-topics  --create --zookeeper 192.168.5.57:2181 --replication-factor 3 --partitions 3 --topic stream
```

- 查看topic(指定多个ip:port逗号分隔)

```
kafka-topics --list --zookeeper 192.168.5.57:2181
```

- 删除topic(指定多个ip:port逗号分隔)

```
kafka-topics --delete --topic test  --zookeeper 192.168.5.57:2181
```

- 生产者发送消息(指定多个ip:port逗号分隔)

```
kafka-console-producer --broker-list 192.168.5.57:9092 --topic test
```

- 消费者接收消息(指定多个ip:port逗号分隔)

```
kafka-console-consumer --topic test --zookeeper 192.168.5.57:2181 --from-beginning
```

- 创建组

```
kafka-console-consumer --zookeeper 192.168.5.59:9092 --topic risen_cloud --group log_group
```

- 查看所有组

```
bin/kafka-consumer-groups.sh  --zookeeper 192.168.10.9:9092 --list
```

- 查看某个组的详细信息

```
bin/kafka-consumer-groups.sh  --zookeeper 192.168.10.9:9092 --group console-consumer-29211 --describe
```
