CREATE OR REPLACE PROCEDURE export_to_csv IS
  CURSOR c_data IS SELECT * FROM your_table;
  v_file UTL_FILE.FILE_TYPE;
  v_dir VARCHAR2(100) := 'DIRECTORY_NAME'; -- replace with your directory name
  v_filename VARCHAR2(100) := 'file.csv';
  v_separator VARCHAR2(1) := ',';
BEGIN
  v_file := UTL_FILE.FOPEN(v_dir, v_filename, 'w', 32767);
  
  -- write column headers to file
  FOR c IN (SELECT COLUMN_NAME FROM USER_TAB_COLUMNS WHERE TABLE_NAME = 'YOUR_TABLE' ORDER BY COLUMN_ID) LOOP
    UTL_FILE.PUT(v_file, c.COLUMN_NAME || v_separator);
  END LOOP;
  UTL_FILE.NEW_LINE(v_file);
  
  -- write data rows to file
  FOR r IN c_data LOOP
    FOR i IN 1..(SELECT COUNT(*) FROM USER_TAB_COLUMNS WHERE TABLE_NAME = 'YOUR_TABLE' ORDER BY COLUMN_ID) LOOP
      UTL_FILE.PUT(v_file, TO_CHAR(r(i)) || v_separator);
    END LOOP;
    UTL_FILE.NEW_LINE(v_file);
  END LOOP;
  
  UTL_FILE.FCLOSE(v_file);
  DBMS_OUTPUT.PUT_LINE('Export complete.');
END;
/
