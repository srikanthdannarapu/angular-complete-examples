# angular-complete-examples

import java.sql.*;

public class CompareNames {

    public static void main(String[] args) throws SQLException {
        // Set up a connection to the first Oracle database
        Connection conn1 = DriverManager.getConnection("jdbc:oracle:thin:@//hostname1:port1/DB1", "username1", "password1");

        // Set up a connection to the second Oracle database
        Connection conn2 = DriverManager.getConnection("jdbc:oracle:thin:@//hostname2:port2/DB2", "username2", "password2");

        // Create a statement to execute the first query on the first database
        Statement stmt1 = conn1.createStatement();
        ResultSet rs1 = stmt1.executeQuery("SELECT employee_name FROM employee_table");

        // Create a statement to execute the second query on the second database
        Statement stmt2 = conn2.createStatement();
        ResultSet rs2 = stmt2.executeQuery("SELECT student_name FROM student_table");

        // Compare the results
        while (rs1.next()) {
            String empName = rs1.getString("employee_name");
            boolean matchFound = false;
            while (rs2.next()) {
                String studentName = rs2.getString("student_name");
                if (empName.equals(studentName)) {
                    matchFound = true;
                    break;
                }
            }
            if (matchFound) {
                System.out.println("Match found: " + empName);
            } else {
                System.out.println("No match found for: " + empName);
            }
            rs2.close();
            stmt2.close();
            stmt2 = conn2.createStatement();
            rs2 = stmt2.executeQuery("SELECT student_name FROM student_table");
        }

        // Close the statement, result set, and connection objects
        rs1.close();
        stmt1.close();
        conn1.close();
        rs2.close();
        stmt2.close();
        conn2.close();
    }
}
