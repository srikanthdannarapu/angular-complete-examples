import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public class DateParser {
    private static final DateTimeFormatter FORMATTER = DateTimeFormatter.ofPattern("dd-MMM-yyyy");

    public static void main(String[] args) {
        String dateString1 = "FOO0320";
        String dateString2 = "TESSSSS0325";
        String year = "2023";
        
        // Parse the first date string
        LocalDate date1 = parseDateString(dateString1, year);
        
        // Parse the second date string
        LocalDate date2 = parseDateString(dateString2, year);
        
        // Get the next day for the first date
        LocalDate nextDay1 = date1.plusDays(1);
        String formattedNextDay1 = nextDay1.format(FORMATTER);
        
        // Get the next day for the second date
        LocalDate nextDay2 = date2.plusDays(1);
        String formattedNextDay2 = nextDay2.format(FORMATTER);
        
        // Output the results
        System.out.println("Date 1: " + date1);
        System.out.println("Next day for date 1: " + formattedNextDay1);
        System.out.println("Date 2: " + date2);
        System.out.println("Next day for date 2: " + formattedNextDay2);
    }
    
    private static LocalDate parseDateString(String dateString, String year) {
        String day = dateString.substring(dateString.length() - 2);
        String month = dateString.substring(dateString.length() - 4, dateString.length() - 2);
        String dateStr = day + "-" + getMonthName(month) + "-" + year;
        return LocalDate.parse(dateStr, FORMATTER);
    }
    
    private static String getMonthName(String month) {
        String[] months = {
            "", "Jan", "Feb", "Mar", "Apr", "May", "Jun",
            "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
        };
        int monthNumber = Integer.parseInt(month);
        return months[monthNumber];
    }
}
