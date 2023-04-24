<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-api</artifactId>
  <version>1.7.30</version>
</dependency>


import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Service
public class MyService {
  private static final Logger LOGGER = LoggerFactory.getLogger(MyService.class);

  public void doSomething() {
    LOGGER.info("Doing something...");
  }
}


<configuration>
  <appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>myapp.log</file>
    <encoder>
      <pattern>%date %-5level [%thread] %logger{35} - %msg%n</pattern>
    </encoder>
  </appender>
  <root level="info">
    <appender-ref ref="FILE" />
  </root>
</configuration>
