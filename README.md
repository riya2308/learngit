java/
│           └── datesandtimes/
│               ├── DatesAndTimesTest.java
│               └── CourseTimeChecker.java




DatesAndTimesTest.java
package datesandtimes;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Scanner;

public class DatesAndTimesTest {

    public static void main(String[] args) {
        // Question 1: Get and print the current minute
        LocalDateTime now = LocalDateTime.now();
        System.out.println("Current minute: " + now.getMinute());

        // Question 2: Input a datetime string and print in a different format
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter date and time in YYYY-MM-DD HH:MM format: ");
        String inputDateTime = scanner.nextLine();
        DateTimeFormatter inputFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");
        LocalDateTime parsedDateTime = LocalDateTime.parse(inputDateTime, inputFormatter);
        DateTimeFormatter outputFormatter = DateTimeFormatter.ofPattern("dd MMMM yyyy 'at' HH:mm");
        System.out.println("Formatted date: " + parsedDateTime.format(outputFormatter));

        // Question 3: Allow user to select input and output format
        System.out.println("Choose input format:");
        System.out.println("1. YYYY-MM-DD HH:MM");
        System.out.println("2. DD/MM/YYYY HH:MM");
        System.out.println("3. MM-DD-YYYY HH:MM");
        int inputChoice = scanner.nextInt();
        scanner.nextLine(); // consume the newline
        String inputPattern = inputChoice == 1 ? "yyyy-MM-dd HH:mm" : inputChoice == 2 ? "dd/MM/yyyy HH:mm" : "MM-dd-yyyy HH:mm";

        System.out.print("Enter date and time in selected format: ");
        inputDateTime = scanner.nextLine();
        inputFormatter = DateTimeFormatter.ofPattern(inputPattern);
        parsedDateTime = LocalDateTime.parse(inputDateTime, inputFormatter);

        System.out.println("Choose output format:");
        System.out.println("1. DD MMMM yyyy 'at' HH:mm");
        System.out.println("2. MM/dd/yyyy HH:mm");
        System.out.println("3. yyyy-MM-dd HH:mm");
        int outputChoice = scanner.nextInt();
        scanner.nextLine(); // consume the newline
        String outputPattern = outputChoice == 1 ? "dd MMMM yyyy 'at' HH:mm" : outputChoice == 2 ? "MM/dd/yyyy HH:mm" : "yyyy-MM-dd HH:mm";

        outputFormatter = DateTimeFormatter.ofPattern(outputPattern);
        System.out.println("Formatted date: " + parsedDateTime.format(outputFormatter));
    }
}



CourseTimeChecker.java
package datesandtimes;

import java.time.LocalDateTime;
import java.time.temporal.ChronoUnit;
import java.util.Scanner;

public class CourseTimeChecker {

    public static void main(String[] args) {
        // Define the end date of the course (adjust this to your actual course end date)
        LocalDateTime courseEndDate = LocalDateTime.of(2024, 12, 31, 23, 59);

        // Question 4: Calculate the number of seconds from now until the end of the course
        LocalDateTime now = LocalDateTime.now();
        long secondsUntilEnd = ChronoUnit.SECONDS.between(now, courseEndDate);
        System.out.println("Seconds until the end of the course: " + secondsUntilEnd);

        // Question 5: Take a datetime input and check if it's within the course period
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter date and time in YYYY-MM-DD HH:MM format: ");
        String inputDateTime = scanner.nextLine();
        LocalDateTime inputDate = LocalDateTime.parse(inputDateTime, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm"));

        LocalDateTime courseStartDate = LocalDateTime.of(2024, 1, 1, 0, 0); // Adjust as needed
        boolean isWithinCoursePeriod = !inputDate.isBefore(courseStartDate) && !inputDate.isAfter(courseEndDate);
        System.out.println("Is within course period: " + isWithinCoursePeriod);
    }
}
