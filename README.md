# angular-complete-examples

import java.sql.*;
import java.util.Arrays;

public class CompareValues {

    public static void main(String[] args) throws SQLException {
        // Set up a connection to your Oracle database
        Connection conn = DriverManager.getConnection("jdbc:oracle:thin:@//localhost:1521/ORCL", "username", "password");

        // Create a prepared statement with a parameterized query
        int numValues = 1000000;
        String sql = "SELECT t.* FROM table_name t WHERE t.concatenated_columns IN (";
        sql += String.join(",", Collections.nCopies(numValues, "?")) + ")";
        PreparedStatement pstmt = conn.prepareStatement(sql);

        // Set the parameter values using a loop
        String[] valuesToCompare = new String[numValues];
        Arrays.fill(valuesToCompare, "value");
        for (int i = 0; i < valuesToCompare.length; i++) {
            pstmt.setString(i + 1, valuesToCompare[i]);
        }

        // Execute the query and process the results
        ResultSet rs = pstmt.executeQuery();
        while (rs.next()) {
            // Retrieve the columns you need from the result set
            String column1 = rs.getString("column1");
            String column2 = rs.getString("column2");
            String column3 = rs.getString("column3");
            // Do something with the retrieved columns
            System.out.println("Match found: " + column1 + ", " + column2 + ", " + column3);
        }

        // Close the statement, result set, and connection objects
        rs.close();
        pstmt.close();
        conn.close();
    }
}
