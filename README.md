@Service
public class LargeTableService {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public LargeTableService(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void processLargeTable() {
        // Set fetch size to control how many rows are retrieved at once
        jdbcTemplate.setFetchSize(10000);

        // Query for the large result set
        jdbcTemplate.query("SELECT * FROM large_table", rs -> {
            // Create a batch update object
            BatchPreparedStatementSetter batchSetter = new MyBatchPreparedStatementSetter(rs);

            // Process the result set in batches
            int rowCount = 0;
            while (rs.next()) {
                // Increment row count
                rowCount++;

                // Add row to batch update object
                batchSetter.setValues(jdbcTemplate.getDataSource().getConnection().prepareStatement("INSERT INTO processed_rows (id) VALUES (?)"), rowCount);

                // If we have processed 10000 rows, execute the batch update and reset the batch setter
                if (rowCount % 10000 == 0) {
                    jdbcTemplate.batchUpdate(batchSetter);

                    // Reset the batch setter
                    batchSetter = new MyBatchPreparedStatementSetter(rs);
                }
            }

            // Execute the final batch update
            jdbcTemplate.batchUpdate(batchSetter);
        });
    }

    private static class MyBatchPreparedStatementSetter implements BatchPreparedStatementSetter {

        private final List<Long> ids = new ArrayList<>();

        public MyBatchPreparedStatementSetter(ResultSet rs) throws SQLException {
            while (rs.next()) {
                ids.add(rs.getLong("id"));
            }
        }

        @Override
        public void setValues(PreparedStatement ps, int i) throws SQLException {
            ps.setLong(1, ids.get(i - 1));
        }

        @Override
        public int getBatchSize() {
            return ids.size();
        }
    }
}
