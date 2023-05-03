DECLARE
   cursor c_employee is
      select employee_id, first_name, last_name
      from employees;
      
   v_result CLOB;
BEGIN
   FOR rec IN c_employee LOOP
      if v_result is null then
         v_result := rec.employee_id || ',' || rec.first_name || ',' || rec.last_name;
      else
         v_result := v_result || ',' || rec.employee_id || ',' || rec.first_name || ',' || rec.last_name;
      end if;
   END LOOP;
   
   -- Output the result to the console or any buffer of your choice
   dbms_output.put_line(v_result);
END;
