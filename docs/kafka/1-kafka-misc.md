# Kafka - 01 - Misc

***

## Listing Kafka Topics

[Source](https://www.baeldung.com/ops/kafka-list-topics)

In a Docker Kafka Container CLI.

### Listing Topics

```console
kafka-topics.sh --list --zookeeper localhost:2181

kafka-topics.sh --list --zookeeper <zookeeper_container_name_in_docker_stack_network>:2181

kafka-topics.sh --list --zookeeper zookeeper-3.5.7:2181
```

### Topic Details

```console
kafka-topics.sh --bootstrap-server=localhost:9092 --describe --topic <topic_name>
```

***
