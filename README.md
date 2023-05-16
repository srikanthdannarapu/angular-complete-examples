import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class EmployeeConverter {

    public static void main(String[] args) {
        // Example list of employees
        List<Employee> employees = List.of(
                new Employee(1, "John"),
                new Employee(2, "Alice"),
                new Employee(3, "Bob"),
                new Employee(4, "Jane"),
                new Employee(5, "Mike"),
                new Employee(6, "Sarah"),
                new Employee(7, "David"),
                new Employee(8, "Emily"),
                new Employee(9, "Tom"),
                new Employee(10, "Linda"),
                new Employee(11, "Chris")
        );

        // Convert the list to a list of lists
        List<List<Employee>> listsOfEmployees = convertToLists(employees, 5);

        // Print the resulting lists of employees
        listsOfEmployees.forEach(System.out::println);
    }

    public static <T> List<List<T>> convertToLists(List<T> originalList, int sublistSize) {
        int numOfLists = (int) Math.ceil((double) originalList.size() / sublistSize);

        return IntStream.range(0, numOfLists)
                .mapToObj(i -> originalList.subList(i * sublistSize, Math.min((i + 1) * sublistSize, originalList.size())))
                .collect(Collectors.toList());
    }

    static class Employee {
        private int id;
        private String name;

        public Employee(int id, String name) {
            this.id = id;
            this.name = name;
        }

        @Override
        public String toString() {
            return "Employee{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    '}';
        }
    }
}
