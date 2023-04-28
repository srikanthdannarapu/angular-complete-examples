SELECT *
FROM (
  SELECT ROW_NUMBER() OVER (ORDER BY <order_column>) AS row_num, t.*
  FROM <table_name> t
)
WHERE row_num = 1000;
