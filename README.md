String query = "DECLARE " +
                "CURSOR c1 IS " +
                "WITH view_test AS (" +
                "  SELECT /*+ PARALLEL */ ft.BANKNET_REF_NUM, ft.FIN_TXN_ID " +
                "  FROM CDF3VPD.FIN_TXN ft " +
                "  WHERE ft.BANKNET_REF_NUM IS NOT NULL " +
                "    AND LENGTH(ft.BANKNET_REF_NUM) = 13 " +
                "    AND created_dt_tm BETWEEN TO_DATE('02/23/2023 00:00:00', 'MM/DD/YYYY HH24:MI:SS') " +
                "    AND TO_DATE('02/23/2023 23:59:59', 'MM/DD/YYYY HH24:MI:SS') " +
                "  ORDER BY ft.FIN_TXN_ID " +
                ") " +
                "SELECT * FROM view_test; " +
                "BEGIN " +
                "  OPEN c1; " +
                "  ? := c1; " +
                "END;";

SqlParameter cursorParam = new SqlOutParameter("c1", OracleTypes.CURSOR, new RowMapper<Object>() {
    @Override
    public Object mapRow(ResultSet rs, int rowNum) throws SQLException {
        Map<String, Object> result = new HashMap<>();
        result.put("BANKNET_REF_NUM", rs.getString("BANKNET_REF_NUM"));
        result.put("FIN_TXN_ID", rs.getLong("FIN_TXN_ID"));
        return result;
    }
});

SimpleJdbcCall jdbcCall = new SimpleJdbcCall(jdbcTemplate).withProcedureName("dbms_sql.return_result")
                .declareParameters(cursorParam);

Map<String, Object> resultMap = jdbcCall.execute(new MapSqlParameterSource());

ResultSet rs = (ResultSet) resultMap.get("c1");

while (rs.next()) {
    String banknetRefNum = rs.getString("BANKNET_REF_NUM");
    Long finTxnId = rs.getLong("FIN_TXN_ID");
    System.out.println("BANKNET_REF_NUM: " + banknetRefNum + ", FIN_TXN_ID: " + finTxnId);
}

rs.close();
