import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.kafka.listener.BatchErrorHandler;
import org.springframework.kafka.listener.SeekToCurrentBatchErrorHandler;

@Configuration
public class KafkaConfiguration {

    @Bean
    public BatchErrorHandler batchErrorHandler() {
        return new SeekToCurrentBatchErrorHandler();
    }
}
