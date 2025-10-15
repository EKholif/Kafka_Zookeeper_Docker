

# ✅ Kafka & Zookeeper with Docker Compose

This setup runs Kafka and Zookeeper locally using Docker Compose.

---

## 📌 1. Docker Compose File yaml

```
version '4.0.1'

kafka  4.0.1
zookeeper 7.9.4


services
  zookeeper
    image confluentinccp-

    container_name zookeeper
    ports
      - 2181:2181
    environment
      ZOOKEEPER_CLIENT_PORT 2181
      ZOOKEEPER_TICK_TIME 2000
   
    ports
      - 9092:9092
    environment
    
      KAFKA_ZOOKEEPER_CONNECT zookeeper2181
      KAFKA_ADVERTISED_LISTENERS  PLAINTEXT://localhost:9092
      KAFKA_LISTENERS PLAINTEXT0.0.0.09092
     


## 📌 2. Start Containers

Run this in the same directory as your `docker-compose.yml`

```bash
docker-compose up -d
```

Check that both services are running

```bash
docker ps
```

You should see

 `zookeeper`
 `kafka`

---

## 📌 3. Create a Topic

```bash

docker exec kafka kafka-topics --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1

```

Verify the topic was created

```bash
docker exec kafka kafka-topics --list --bootstrap-server localhost:9092
```


## 📌 4. Produce Messages (Producer)

```bash
docker exec -it kafka kafka-console-producer --topic my-topic --bootstrap-server localhost:9092
```

Type a message and press Enter, e.g.

```
hello kafka
```

---

## 📌 5. Consume Messages (Consumer)

Open a new terminal and run

```bash
docker exec -it kafka kafka-console-consumer --topic my-topic --bootstrap-server localhost:9092 --from-beginning
```

You should see any messages you produced.

---

## 📌 6. Stop Services

To stop and remove the containers

```bash
docker-compose down
```

To also remove volumes

```bash
docker-compose down -v
```

---

## 📌 7. Spring Boot Configuration Example

Add to `application.properties`

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```


