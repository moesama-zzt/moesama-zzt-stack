通过docker拉取kafla镜像
```docker pull apache/kafka:3.8.0```

运行kafka容器
```docker run -itd -p 9092:9092 --name kafka apache/kafka:3.8.0```

进入kafka容器内部
```docker exec -it kafka bash```

进入kafka服务所在目录
```cd /opt/kafka/bin```

发布主题
```./kafka-topics.sh --create --topic v2space-topic --bootstrap-server localhost:9092```

```Created topic v2space-topic.```

查看主题描述
```./kafka-topics.sh --describe --topic v2space-topic --bootstrap-server localhost:9092```

```..
Topic: v2space-topic    Partition: 0    Leader: 1       Replicas: 1     Isr: 1  Elr:    LastKnownElr:
```

kafka生产者
```./kafka-console-producer.sh --topic v2space-topic --bootstrap-server localhost:9092```

kafka消费者
```./kafka-console-consumer.sh --topic v2space-topic --bootstrap-server localhost:9092```

![[kafka_view.png]]