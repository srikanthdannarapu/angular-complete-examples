import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class EmployeeReader {

    @Autowired
    private Environment environment;

    public List<Employee> getEmployees() {
        List<Employee> employees = new ArrayList<>();
        int index = 0;

        while (true) {
            String idKey = "yaml.employees[" + index + "].id";
            String nameKey = "yaml.employees[" + index + "].name";
            String ageKey = "yaml.employees[" + index + "].age";
            String departmentKey = "yaml.employees[" + index + "].department";

            String id = environment.getProperty(idKey);
            String name = environment.getProperty(nameKey);
            String age = environment.getProperty(ageKey);
            String department = environment.getProperty(departmentKey);

            if (id == null || name == null || age == null || department == null) {
                break;
            }

            Employee employee = new Employee();
            employee.setId(Integer.parseInt(id));
            employee.setName(name);
            employee.setAge(Integer.parseInt(age));
            employee.setDepartment(department);

            employees.add(employee);

            index++;
        }

        return employees;
    }
}
