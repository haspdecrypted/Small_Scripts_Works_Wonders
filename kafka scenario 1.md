<h1> Scenario </h1>

I have five users for xyz who are getting some subscription like mobile, device and cellular plans to any of these or many out of these three options. Now using kafka streams I want to get the real time data for all these subscriptions and divided it into different topics assigned to the user and its end data and how it is consumed. 

For example: 

user_id- user101, user102, user103, user104, user105. 
subscription available: mobile, device, cellular plan.

user101 gets device and cellular plan and user102 gets mobile likewise use some dummy scenarios and show me the code in java. 

<h3> Solution </h3>
To implement this real-time data processing scenario using Kafka Streams in Java, you can follow these steps:

1. Create a Kafka topic to produce subscription data events. Each event should include user information (e.g., user_id) and the subscription type (e.g., mobile, device, cellular plan).

2. Set up a Kafka Streams application to consume the subscription data from the topic and route it to different topics based on the user's subscriptions.

Here's an example code snippet that demonstrates this scenario:

```java
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.apache.kafka.common.serialization.StringSerializer;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.StreamsBuilder;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.Consumed;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.Produced;

import java.util.Properties;

public class SubscriptionProcessor {

    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(StreamsConfig.APPLICATION_ID_CONFIG, "subscription-processor");
        props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        StreamsBuilder builder = new StreamsBuilder();

        // Create a stream of subscription data from the input topic
        KStream<String, String> subscriptionStream = builder.stream("subscription-topic",
                Consumed.with(Serdes.String(), Serdes.String()));

        // Process the stream and route data to user-specific topics based on subscriptions
        subscriptionStream
                .filter((key, value) -> value != null)
                .foreach((key, value) -> {
                    String[] parts = value.split(",");
                    String user = parts[0];
                    String subscription = parts[1];

                    // Create a new topic for each user's subscription
                    String userTopic = user + "-" + subscription;

                    // Produce the data to the user-specific topic
                    produceToTopic("user-subscription-topic", key, value, userTopic);
                });

        KafkaStreams streams = new KafkaStreams(builder.build(), props);
        streams.start();
    }

    private static void produceToTopic(String topic, String key, String value, String newTopic) {
        Properties producerProps = new Properties();
        producerProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        producerProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        producerProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());

        // Create a Kafka producer
        org.apache.kafka.clients.producer.Producer<String, String> producer = new org.apache.kafka.clients.producer.KafkaProducer<>(producerProps);

        // Produce the data to the new topic
        ProducerRecord<String, String> record = new ProducerRecord<>(newTopic, key, value);
        producer.send(record);

        producer.close();
    }
}
```

In this code:

- We set up a Kafka Streams application that consumes data from the "subscription-topic".
- For each incoming subscription, we split it into user and subscription type.
- We create a new topic for each user's subscription (e.g., "user101-device", "user102-mobile") and produce the data to those topics.

Make sure to adjust the Kafka broker configuration (`bootstrap.servers`) and topic names as per your environment. Also, ensure you have the Kafka dependencies set up in your project.

<h3> In Java Springboot </h3>

To create a Java Spring Boot application that uses Kafka to process real-time subscription data and routes it to different topics based on user subscriptions, you can follow these steps:

1. Set up a Spring Boot project with the necessary dependencies for Kafka, such as `spring-kafka` and `spring-boot-starter`.

2. Create a Kafka producer and consumer configuration.

3. Implement a Kafka message listener to consume subscription data.

4. Process the data and produce it to different topics based on user subscriptions.

Here's a sample Java Spring Boot application that demonstrates this scenario:

```java
import org.apache.kafka.clients.admin.NewTopic;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.kafka.annotation.EnableKafka;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Service;

@SpringBootApplication
@EnableKafka
public class SubscriptionProcessorApplication {

    public static void main(String[] args) {
        SpringApplication.run(SubscriptionProcessorApplication.class, args);
    }

    @Bean
    public NewTopic userSubscriptionTopic() {
        return new NewTopic("user-subscription-topic", 1, (short) 1);
    }

    @Service
    public class SubscriptionService {

        private final KafkaTemplate<String, String> kafkaTemplate;

        public SubscriptionService(KafkaTemplate<String, String> kafkaTemplate) {
            this.kafkaTemplate = kafkaTemplate;
        }

        // Kafka message listener to consume subscription data
        @KafkaListener(topics = "subscription-topic", groupId = "subscription-group")
        public void processSubscription(String subscription) {
            String[] parts = subscription.split(",");
            String user = parts[0];
            String subscriptionType = parts[1];

            // Create a new topic for each user's subscription
            String userTopic = user + "-" + subscriptionType;

            // Produce the data to the user-specific topic
            kafkaTemplate.send(userTopic, subscription);
        }
    }
}
```

In this code:

- We create a Spring Boot application with Kafka support enabled (`@EnableKafka`).
- We define a Kafka listener method `processSubscription` that consumes subscription data from the "subscription-topic" and routes it to user-specific topics based on subscription types.
- The `user-subscription-topic` is created as a bean using `NewTopic`, and it's used to send processed data.

Make sure to configure your Kafka broker details in the `application.properties` file or via configuration beans. Additionally, set up Kafka topics and adjust the topic names as per your requirements.

Remember to include the necessary Kafka dependencies in your `pom.xml` or `build.gradle` file to use Kafka functionality in your Spring Boot application.
