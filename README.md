public List<Object[]> getCursorResults() {
    String sql = "DECLARE " +
                 "  CURSOR c1 IS " +
                 "    WITH view_test AS (" +
                 "      SELECT /*+ PARALLEL */ ft.BANKNET_REF_NUM, ft.FIN_TXN_ID " +
                 "      FROM CDF3VPD.FIN_TXN ft " +
                 "      WHERE ft.BANKNET_REF_NUM is not null " +
                 "      AND (LENGTH(ft.BANKNET_REF_NUM) = 13) " +
                 "      AND created_dt_tm BETWEEN TO_DATE('02/23/2023 00:00:00', 'MM/DD/YYYY HH24:MI:SS') " +
                 "      AND TO_DATE('02/23/2023 23:59:59', 'MM/DD/YYYY HH24:MI:SS') " +
                 "      ORDER BY ft.FIN_TXN_ID " +
                 "    ) " +
                 "    SELECT * FROM view_test; " +
                 "  v_BANKNET_REF_NUM VARCHAR2(20); " +
                 "  v_FIN_TXN_ID NUMBER; " +
                 "BEGIN " +
                 "  OPEN c1; " +
                 "  LOOP " +
                 "    FETCH c1 INTO v_BANKNET_REF_NUM, v_FIN_TXN_ID; " +
                 "    EXIT WHEN c1%NOTFOUND; " +
                 "    DBMS_OUTPUT.PUT_LINE('BANKNET_REF_NUM: ' || v_BANKNET_REF_NUM || ', FIN_TXN_ID: ' || v_FIN_TXN_ID); " +
                 "  END LOOP; " +
                 "  CLOSE c1; " +
                 "END;";
    
    List<Object[]> results = new ArrayList<>();
    jdbcTemplate.execute(sql, (CallableStatementCallback<Void>) cs -> {
        cs.execute();
        ResultSet rs = (ResultSet) cs.getObject(1);
        while (rs.next()) {
            Object[] row = new Object[2];
            row[0] = rs.getObject("BANKNET_REF_NUM");
            row[1] = rs.getObject("FIN_TXN_ID");
            results.add(row);
        }
        return null;
    });
    return results;
}
