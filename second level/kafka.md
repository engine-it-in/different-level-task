<!-- TOC -->
* [Kafka](#kafka)
  * [General](#general)
    * [Масштабируемость](#масштабируемость)
    * [Модель обработки данных](#модель-обработки-данных)
    * [Гарантия доставки](#гарантия-доставки)
    * [Хранение сообщений](#хранение-сообщений)
    * [Управление и мониторинг](#управление-и-мониторинг)
    * [Использование в реальных сценариях](#использование-в-реальных-сценариях)
  * [Components](#components)
    * [Topics](#topics)
      * [Partitions](#partitions)
    * [Producers](#producers)
    * [Consumers](#consumers)
    * [Brokers](#brokers)
    * [Zookeeper](#zookeeper)
  * [Diagram](#diagram)
  * [Practice](#practice)
    * [Preparing](#preparing)
    * [Producer/Consumer](#producerconsumer)
    * [Properties](#properties)
      * [Json](#json)
      * [Avro](#avro)
<!-- TOC -->

# Kafka

## General

Распределенная платформа, предназначенная для обработки, хранения и передачи потоковых данных в реальном времени.
Работает по модели "издатель-подписчик" (publish-subscribe) и предоставляет передачу сообщений между несколькими
производителями и несколькими потребителями.

### Масштабируемость

Масштабируется горизонтально.

### Модель обработки данных

Сообщения сохраняются в топиках. Потребители читают сообщения из топиков и могут "перематывать" их назад, чтобы
перечитать. Сообщения сохраняются на диск и могут храниться в течение длительного времени независимо от того, были они
прочитаны потребителями или нет.

### Гарантия доставки

Предоставляет гарантии:

- "at least once" (по крайней мере один раз);
- "at most once" (не более одного раза);
- "exactly once" (точно один раз) для доставки сообщений, что подходит для систем, где требуется надежная обработка
  потоков данных.

### Хранение сообщений

Хранит данные на диске, что позволяет хранить сообщения в течение длительного времени и обеспечивает быстрый доступ к
ним.

### Управление и мониторинг

Имеет несколько сторонних инструментов для мониторинга и управления кластером, таких как Kafka Manager,
Confluent Control Center и другие.

### Использование в реальных сценариях

Идеален для сценариев, где требуется обработка больших потоков данных, таких как логирование, трекинг
активности пользователей, агрегация данных в реальном времени и интеграция с большими системами аналитики данных.

## Components

### Topics

Топики являются категориями, в которых производители публикуют сообщения, а потребители считывают их. Они разделены на
несколько партиций, каждая из которых может быть реплицирована для обеспечения отказоустойчивости.

#### Partitions

Партиции - это физически разделенные логические области внутри топика в Apache Kafka. Каждый топик может быть разделен
на несколько партиций, причем каждая партиция имеет набор упорядоченных и неизменяемых сообщений.

Партицирование топика на несколько партиций позволяет Kafka горизонтально масштабировать хранение и обработку данных.
Каждая партиция может быть распределена по разным брокерам в Kafka-кластере. Это обеспечивает балансировку нагрузки и
распределение данных между брокерами.

Производители записывают сообщения в определенную партицию, соответствующую ключу записи или используют алгоритмы
вычисления хэша для распределения сообщений по партициям.

Важно отметить, что каждая партиция внутри одного топика имеет уникальное смещение (offset), которое используется для
идентификации позиции в сообщениях внутри партиции.

### Producers

Отвечают за создание и отправку сообщений в топики. Они могут отправлять сообщения асинхронно или синхронно.

### Consumers

Подписываются на топики и читают сообщения из них. Потребители могут обрабатывать сообщения, включая параллельную
обработку с помощью группы потребителей (могут читать параллельно из разных партиций для повышения пропускной
способности и масштабируемости). Потребители могут читать сообщения с определенным смещением и могут сохранять свое
смещение для продолжения чтения с определенной точки после перезапуска.

### Brokers

Это серверы в Kafka-кластере, которые хранят и управляют топиками и сообщениями. Каждый брокер отвечает за несколько
партиций в различных топиках.

### Zookeeper

Координационный сервис, который используется для управления состоянием брокеров, хранения метаданных и обеспечения
отказоустойчивости Kafka-кластера.

## Diagram

- Producer -> (produces message) -> Topic
- Topic -> (saves message) -> Partition
- Partition -> (stores data in) -> Broker
- Consumer -> (reades message from) -> Topic

## Practice

См.проект demo-kafka

### Preparing

1. Установить Kafka (`brew install kafka`)
2. Создать топик:
   `kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic nosbuka`
3. Установить время жизни сообщений:
   `kafka-configs --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name nosbuka --add-config retention.ms=60000`

### Producer/Consumer

KafkaTemplate автоматически конфигурируется Spring Boot из значений properties

```java

@Service
@Log4j2
@RequiredArgsConstructor
public class KafkaProducerService {

    private final KafkaTemplate<String, KafkaMessage> kafkaTemplate;
    private int count = 1;

    @Scheduled(initialDelay = 3000, fixedRate = 5000)
    public void produce() {
        log.info("producing message {}", count);
        kafkaTemplate.send(
                new ProducerRecord<>("nosbuka", new KafkaMessage(RandomStringUtils.randomAlphanumeric(20)))
        ).whenComplete((sendResult, throwable) -> {
            if (throwable != null) {
                log.error("ERROR with message: {}", throwable.getMessage());
            }
            log.info(sendResult.toString());
        });
        count++;
    }
}
```

@KafkaListener(topics = "nosbuka", groupId = "demo-kafka-application") - аннотация, которая делает потребителя.
Метод может иметь сигнатуру ConsumerRecord<String, KafkaMessage> consumed, позволяющую получить доп.метаданные
сообщения.

```java

@Service
@Log4j2
@RequiredArgsConstructor
public class KafkaConsumerService {

    @KafkaListener(topics = "nosbuka")
    public void consume(ConsumerRecord<String, KafkaMessage> consumed) {
        log.info("Consumed message with offset {} with value {}",
                consumed.offset(),
                consumed.value());
    }
}
```

### Properties

```yaml
spring:
  application:
    name: demo-kafka-application
  kafka:
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: io.confluent.kafka.serializers.KafkaAvroSerializer
    consumer:
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: io.confluent.kafka.serializers.KafkaAvroDeserializer
      enable-auto-commit: true
      auto-commit-interval: 1000
      # Если указать тут, то не нужно будет указывать в @KafkaListener аннотации
      # Kafka использует это для отслеживания прогресса, то есть, какие сообщения уже были обработаны и какие еще
      # предстоит обработать.
      group-id: ${spring.application.name}
      # Нужно указывать чтоб кафка не боялась десериализовывать сообщения (применимо к Json)
      properties:
        spring.json.trusted.packages: ru.nosbuka.demo.kafka.app.model
    template:
      default-topic: nosbuka
    bootstrap-servers: localhost:9092
    properties:
      schema.registry.url: http://localhost:8081
```

#### Json

---

Add dependency

```xml

<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```

Add Serializer/Deserializer

```yaml
producer:
  key-serializer: org.apache.kafka.common.serialization.StringSerializer
  value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
consumer:
  key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
```

Create model

```java
public record KafkaMessage(String message) {
}
```

#### Avro

---

Add Dependencies

```xml

<dependencies>
    <dependency>
        <groupId>org.apache.avro</groupId>
        <artifactId>avro</artifactId>
        <version>1.11.3</version>
    </dependency>
    <dependency>
        <groupId>io.confluent</groupId>
        <artifactId>kafka-avro-serializer</artifactId>
        <version>5.3.0</version>
    </dependency>
</dependencies>
```

Add schema:

```avsc
{
  "namespace": "ru.nosbuka.demo.kafka.app.model",
  "type" : "record",
  "name" : "KafkaMessage",
  "fields" : [ {
    "name" : "message",
    "type" : "string"
  } ]
}
```

Add maven plugin and generate class
Плагин генерирует класс из модели .avsc

```xml

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.avro</groupId>
            <artifactId>avro-maven-plugin</artifactId>
            <version>${avro.version}</version>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <goals>
                        <goal>schema</goal>
                    </goals>
                    <configuration>
                        <sourceDirectory>${project.basedir}/src/main/resources/avro</sourceDirectory>
                        <outputDirectory>${project.basedir}/src/main/java</outputDirectory>
                        <stringType>String</stringType>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

Add properties for consumer/producer

```yaml
producer:
  key-serializer: org.apache.kafka.common.serialization.StringSerializer
  value-serializer: io.confluent.kafka.serializers.KafkaAvroSerializer
consumer:
  key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  value-deserializer: io.confluent.kafka.serializers.KafkaAvroDeserializer
```

Add properties for schema registry url

Schema Registry - это сервис, который обеспечивает хранение и извлечение версий схем Avro. При работе с сериализованными
данными, особенно в распределенных системах, как Kafka, очень важно иметь точное представление о структуре данных. В
рамках Avro схемы определяют структуру данных. Однако, с течением времени схемы данных могут меняться. Чтобы
обрабатывать эти изменения корректно, поддержка нескольких версий схем становится критично важной. Именно эту
функциональность предоставляет Schema Registry.

Confluent Schema Registry предоставляет RESTful интерфейс для хранения и извлечения Avro-схем. Он сохраняет версию
каждой схемы и позволяет извлекать схему по ID. Каждое сообщение, которое отправляется через Kafka, будет содержать
идентификатор схемы, а не саму схему, что повышает эффективность передачи данных.

```yaml
spring:
  kafka:
    properties:
      schema.registry.url: http://localhost:8081
```