import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;

import javax.sql.DataSource;
import java.util.List;

@Service
public class LargeTableService {

    private final JdbcTemplate jdbcTemplate;

    public LargeTableService(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    public List<MyRecord> getRecords(int limit) {
        String sql = "SELECT * FROM (SELECT * FROM mytable WHERE some_column = 'some_value' AND ROWNUM <= ?) WHERE ROWNUM >= 1";
        return jdbcTemplate.query(sql, new Object[]{limit}, new MyRecordRowMapper());
    }
}
