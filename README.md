import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.kafka.annotation.EnableKafka;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.kafka.config.ConcurrentKafkaListenerContainerFactory;
import org.springframework.kafka.core.DefaultKafkaConsumerFactory;
import org.springframework.kafka.listener.ContainerProperties;
import org.springframework.kafka.listener.KafkaMessageListenerContainer;
import org.springframework.kafka.listener.SeekToCurrentBatchErrorHandler;
import org.springframework.kafka.support.Acknowledgment;
import org.springframework.kafka.support.KafkaHeaders;
import org.springframework.stereotype.Component;

import java.util.Properties;

@SpringBootApplication
@EnableKafka
public class KafkaBatchConsumerApplication {

    public static void main(String[] args) {
        SpringApplication.run(KafkaBatchConsumerApplication.class, args);
    }

    @Component
    public class KafkaBatchListener {

        @Value("${spring.kafka.bootstrap-servers}")
        private String bootstrapServers;

        @Value("${spring.kafka.consumer.group-id}")
        private String groupId;

        @Value("${spring.kafka.consumer.topic}")
        private String topic;

        @KafkaListener(topics = "${spring.kafka.consumer.topic}", containerFactory = "kafkaBatchListenerContainerFactory")
        public void listen(ConsumerRecords<String, String> records, Acknowledgment acknowledgment) {
            for (ConsumerRecord<String, String> record : records) {
                // Process the record here
                System.out.println("Received message: " + record.value());
            }
            acknowledgment.acknowledge();
        }

        public KafkaMessageListenerContainer<String, String> kafkaMessageListenerContainer() {
            Properties props = new Properties();
            props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
            props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
            props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
            props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");

            DefaultKafkaConsumerFactory<String, String> consumerFactory = new DefaultKafkaConsumerFactory<>(props);

            ContainerProperties containerProperties = new ContainerProperties(topic);
            containerProperties.setBatchErrorHandler(new SeekToCurrentBatchErrorHandler());

            KafkaMessageListenerContainer<String, String> container = new KafkaMessageListenerContainer<>(consumerFactory, containerProperties);
            return container;
        }

        public ConcurrentKafkaListenerContainerFactory<String, String> kafkaBatchListenerContainerFactory() {
            ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
            factory.setConsumerFactory(new DefaultKafkaConsumerFactory<>(consumerConfigs()));
            factory.setBatchListener(true);
            factory.setBatchErrorHandler(new SeekToCurrentBatchErrorHandler());
            return factory;
        }

        private Map<String, Object> consumerConfigs() {
            Map<String, Object> props = new HashMap<>();
            props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
            props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
            props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
            props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
            return props;
        }
    }
}
