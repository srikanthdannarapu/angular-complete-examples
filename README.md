import java.sql.*;
import java.util.HashSet;

public class CompareNames {
    public static void main(String[] args) throws SQLException {
        // Set up a connection to the first Oracle database
        Connection conn1 = DriverManager.getConnection("jdbc:oracle:thin:@//hostname1:port1/DB1", "username1", "password1");

        // Set up a connection to the second Oracle database
        Connection conn2 = DriverManager.getConnection("jdbc:oracle:thin:@//hostname2:port2/DB2", "username2", "password2");

        // Create a statement to execute the first query on the first database
        Statement stmt1 = conn1.createStatement();
        ResultSet rs1 = stmt1.executeQuery("SELECT employee_name FROM employee_table");

        // Create a set to hold the student names
        HashSet<String> studentNames = new HashSet<>();

        // Create a statement to execute the query to retrieve all student names from the second database
        Statement stmt2 = conn2.createStatement();
        ResultSet rs2 = stmt2.executeQuery("SELECT student_name FROM student_table");

        // Load all the student names into the set
        while (rs2.next()) {
            String studentName = rs2.getString("student_name");
            studentNames.add(studentName);
        }

        // Set up pagination parameters for the employee names
        int batchSize = 10000;
        int offset = 0;

        // Compare the results
        while (true) {
            // Create a new statement for each batch of employee names
            Statement stmt3 = conn1.createStatement();
            stmt3.setMaxRows(batchSize);
            ResultSet rs3 = stmt3.executeQuery("SELECT employee_name FROM employee_table ORDER BY employee_name OFFSET " + offset);

            // Process the current batch of employee names
            while (rs3.next()) {
                String empName = rs3.getString("employee_name");
                if (studentNames.contains(empName)) {
                    System.out.println("Match found: " + empName);
                } else {
                    System.out.println("No match found for: " + empName);
                }
            }

            // Close the result set and statement for the current batch
            rs3.close();
            stmt3.close();

            // Move to the next batch of employee names, or exit the loop if there are no more records
            offset += batchSize;
            if (rs3.getRow() < batchSize) {
                break;
            }
        }

        // Close the result sets and statements for the employee and student queries, and the connection objects
        rs1.close();
        stmt1.close();
        rs2.close();
        stmt2.close();
        conn1.close();
        conn2.close();
    }
}
