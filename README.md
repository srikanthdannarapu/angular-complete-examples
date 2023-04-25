public List<MyRecord> getRecordsFromDb1(int offset, int limit) {
    String sql = "SELECT * FROM mytable WHERE some_column = 'some_value' AND ROWNUM > ? AND ROWNUM <= ?";
    return jdbcTemplate1.query(sql, new Object[]{offset, offset + limit}, new MyRecordRowMapper());
}


int limit = 10000;
int offset = 0;
for (int i = 0; i < 10; i++) {
    List<MyRecord> recordsFromDb1 = service.getRecordsFromDb1(offset, limit);
    offset += limit;
}
