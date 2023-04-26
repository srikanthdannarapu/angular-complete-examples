import org.springframework.jdbc.core.JdbcTemplate;
import java.util.Arrays;
import java.util.List;

public class MyDAO {

    private JdbcTemplate jdbcTemplate;

    public MyDAO(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<MyRecord> getRecordsWithIds(List<Long> ids) {
        String sql = "SELECT * FROM myTable WHERE column1 IN (" +
                     String.join(",", Arrays.asList(new String[new ids.size()]).toArray(new String[0])) +
                     ")";
        return jdbcTemplate.query(sql, ids.toArray(), new MyRecordRowMapper());
    }

}
