# Kafka - 01 - Misc

---

## Listing Kafka Topics

[Source](https://www.baeldung.com/ops/kafka-list-topics)

In a Docker Kafka Container CLI.

### Listing Topics

```console
./kafka-topics.sh --list --zookeeper localhost:2181

./kafka-topics.sh --list --zookeeper <zookeeper_container_name_in_docker_stack_network>:2181

./kafka-topics.sh --list --zookeeper zookeeper-3.5.7:2181
```

### Topic Details

```console
./kafka-topics.sh --bootstrap-server=localhost:9092 --describe --topic <topic_name>
```

---

### View message

[How to view kafka message](https://stackoverflow.com/questions/44239027/how-to-view-kafka-message)

```console
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic <topic name> (--from-beginning)
```

---

## Folder

E.g. of folder where Kafka may reside on a Linux machine:

```txt
/usr/local/kafka
```

---

## Notes

zookeeper + kafka -> docker

[SRC: - bitnami kafka](https://hub.docker.com/r/bitnami/kafka/)

Pull kafka image:

```console
docker pull bitnami/kafka:2.4.1
```

Pull zookeeper image:

```console
docker pull bitnami/zookeeper:3.5.7
```

Create a network:

```console
docker network create kfk-network-001 --driver bridge
```

Launch the Zookeeper server instance:

```console
docker run -d --name zookeeper-3.5.7-001 --network kfk-network-001 -e ALLOW_ANONYMOUS_LOGIN=yes bitnami/zookeeper:3.5.7
```

Launch the Kafka server instance:

```console
docker run -d --name kafka-2.4.1-001 -p 9092:9092 -e KAFKA_BROKER_ID=1 --network kfk-network-001 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 -e ALLOW_PLAINTEXT_LISTENER=yes -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 -e KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper-3.5.7-001:2181 bitnami/kafka:2.4.1
```

---

## Test/Debug install

From cli in docker container

List all topic:

```console
kafka-topics.sh --list --zookeeper zookeeper-3.5.7-001:2181
```


Remove all topic:

```console
kafka-topics.sh --delete --topic '[ a-z ].*' --zookeeper zookeeper-3.5.7-001:2181
```

---

## Broken install

On local broken install, remove everything, network as well after removing kafka and zookeeper container (docker network rm kfk-network-001),
then reinstall all with above commands.

From Docker container console, list all Kafka topics:

```console
cd /opt/bitnami/kafka
bin/kafka-topics.sh --list --zookeeper zookeeper-3.5.7-001:2181
```

SRC:

- [kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc](https://www.confluent.io/blog/kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc/)  
- [the-ultimate-introduction-to-kafka-with-javascript](https://soshace.com/the-ultimate-introduction-to-kafka-with-javascript/)

From WSL console in a local folder with Kafka local unzipped archive files (e.g. /mnt/p/kfk/kafka_2.12-2.4.0/kafka_2.12-2.4.0/bin)

Create topic:

```console
./kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```

List topic:

```console
./kafka-topics.sh --list --bootstrap-server localhost:9092
```

Produce on test topic:

```console
./kafka-console-producer.sh --broker-list lohost:9092 --topic test
```

Consume test topic:

```console
./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```

---

## Docker

docker container kafka -> terminal

cd /opt/bitnami/kafka/bin

List topics:

```console
/kafka-topics.sh --list --bootstrap-server localhost:9092
```

List consumer groups:

```console
/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```

Describe consumer group to check LAG, LAG = diff (LOG-END-OFFSET - CURRENT-OFFSET) = amount of not consumed message:

```console
/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group <group name>
```

List all messages in a topic, from beginning:

```console
/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic <topic name> --from-beginning
```

---
