List<String> names = Arrays.asList("John", "Jane", "Bob");

// Convert String values to SQL VARCHAR2 type
List<Object> nameParams = names.stream()
                                .map(name -> new SqlParameterValue(Types.VARCHAR, name))
                                .collect(Collectors.toList());

String sql = "SELECT * FROM myTable WHERE column1 IN (:names)";
MapSqlParameterSource parameters = new MapSqlParameterSource();
parameters.addValue("names", nameParams);

List<MyRecord> records = jdbcTemplate.query(sql, parameters, new MyRecordRowMapper());
