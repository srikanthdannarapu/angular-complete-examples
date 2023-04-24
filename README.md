import org.springframework.jdbc.core.RowMapper;

import java.sql.ResultSet;
import java.sql.SQLException;

public class MyRecordRowMapper implements RowMapper<MyRecord> {

    @Override
    public MyRecord mapRow(ResultSet rs, int rowNum) throws SQLException {
        MyRecord record = new MyRecord();
        record.setId(rs.getInt("id"));
        record.setColumn1(rs.getString("column1"));
        record.setColumn2(rs.getString("column2"));
        // Map other columns as required
        return record;
    }
}
