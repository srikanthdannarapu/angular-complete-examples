import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import javax.sql.DataSource;

@Configuration
public class AppConfig {

    @Bean
    public DataSource dataSource1() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("oracle.jdbc.driver.OracleDriver");
        dataSource.setUrl("jdbc:oracle:thin:@//hostname1:port1/service_name1");
        dataSource.setUsername("username1");
        dataSource.setPassword("password1");
        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate1() {
        return new JdbcTemplate(dataSource1());
    }

    @Bean
    public DataSource dataSource2() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("oracle.jdbc.driver.OracleDriver");
        dataSource.setUrl("jdbc:oracle:thin:@//hostname2:port2/service_name2");
        dataSource.setUsername("username2");
        dataSource.setPassword("password2");
        return dataSource;
    }

    @Bean
    public JdbcTemplate jdbcTemplate2() {
        return new JdbcTemplate(dataSource2());
    }

}
