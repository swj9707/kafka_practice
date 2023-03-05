# kafka_practice
- Apache kafka 를 활용한 메시징 발행 및 조회 테스트 프로그램
# How to use
- docker-compose.yml 파일을 통해 기초 셋업을 한다. 
```yaml
version: "3.8"

services:
  zookeeper:
    container_name: my_zookeeper
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_SERVER_ID: 1
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
      ZOOKEEPER_INIT_LIMIT: 5
      ZOOKEEPER_SYNC_LIMIT: 2
    ports:
      - "22181:2181"
  kafka:
    container_name: my_kafka
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - "29092:29092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: "zookeeper:2181"
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS: 0

```
- testTopic 으로 topic 하나를 발행한다.
```shell
$ docker-compose exec kafka kafka-topics --create --topic testTopic --bootstrap-server kafka:9092 --partitions 3
```
- 직접 메시지를 전달 해 본다.  
![image](https://user-images.githubusercontent.com/36991763/222958567-db788735-1087-49a9-9b13-f36217c93c1e.png)
![image](https://user-images.githubusercontent.com/36991763/222958582-723a7459-894c-43ea-9790-9c15e1a9cc66.png)
