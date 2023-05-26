dependencies {
    // Spring Boot Starter
    implementation 'org.springframework.boot:spring-boot-starter'

    // Spring Cloud Stream with Kafka Binder
    implementation 'org.springframework.cloud:spring-cloud-stream-binder-kafka'

    // Axon Framework
    implementation 'org.axonframework:axon-spring-boot-starter'

    // Axon Outbox
    implementation 'org.axonframework.extensions.springcloud:axon-spring-cloud-starter-outbox'

    // Apache Kafka Client
    implementation 'org.apache.kafka:kafka-clients'

    // Spring Kafka
    implementation 'org.springframework.kafka:spring-kafka'

    // Axon Server
    implementation 'org.axonframework:axon-server-connector'

    // Axon Kafka Outbox
    implementation 'org.axonframework.extensions.kafka:axon-kafka-outbox'

    // Axon Spring Cloud Extension
    implementation 'org.axonframework.extensions.springcloud:axon-spring-cloud-starter'

    // Spring Boot Test (optional, for testing)
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}







import org.springframework.messaging.Message;
import org.springframework.messaging.MessageHeaders;
import org.springframework.messaging.handler.annotation.Payload;
import org.springframework.stereotype.Component;

@Component
public class AxonMessageListener {

    private final KafkaOutboxTemplate kafkaOutboxTemplate;
    private final SpringCloudOutboxEventPublisher outboxEventPublisher;

    @Autowired
    public AxonMessageListener(KafkaOutboxTemplate kafkaOutboxTemplate,
                               SpringCloudOutboxEventPublisher outboxEventPublisher) {
        this.kafkaOutboxTemplate = kafkaOutboxTemplate;
        this.outboxEventPublisher = outboxEventPublisher;
    }

    @KafkaListener(topics = "<axon-topic>")
    public void handleMessage(@Payload Message<Employee> message) {
        Employee employee = message.getPayload();
        MessageHeaders headers = message.getHeaders();

        // Process the message from the Axon topic
        System.out.println("Received message from Axon topic:");
        System.out.println("Payload: " + employee);
        System.out.println("Headers: " + headers);
        System.out.println();

        // Create a KafkaOutboxMessage and publish it to the Outbox
        KafkaOutboxMessage outboxMessage = KafkaOutboxMessage.newBuilder()
                .setKey(headers.getId().toString())
                .setPayload(employee)
                .setTopic("<target-topic>") // Specify the target topic for publishing
                .build();
        kafkaOutboxTemplate.send(outboxMessage);

        // Publish the Outbox event
        outboxEventPublisher.publish(outboxMessage);
    }
}





import org.axonframework.extensions.springcloud.outbox.SpringCloudOutboxEventPublisher;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class OutboxEventPublisherConfig {

    @Bean
    public SpringCloudOutboxEventPublisher outboxEventPublisher(AxonServerEventStore axonServerEventStore) {
        return new SpringCloudOutboxEventPublisher(axonServerEventStore);
    }
}


import org.axonframework.extensions.kafka.outbox.KafkaOutboxTemplate;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class KafkaOutboxConfig {

    @Bean
    public KafkaOutboxTemplate kafkaOutboxTemplate(KafkaTemplate<String, byte[]> kafkaTemplate) {
        return new KafkaOutboxTemplate(kafkaTemplate);
    }
}



import org.axonframework.extensions.kafka.outbox.KafkaOutboxTemplate;
import org.springframework.kafka.core.KafkaTemplate;

import org.axonframework.extensions.springcloud.outbox.SpringCloudOutboxEventPublisher;
import org.axonframework.extensions.springcloud.store.AxonServerEventStore;

