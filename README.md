import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVRecord;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;

import java.io.BufferedReader;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;

public class WebInteraction {
    public static void main(String[] args) throws InterruptedException {
        BufferedReader br = new BufferedReader(new FileReader("path_to_your_csv_file"));
        Iterable<CSVRecord> records = CSVFormat.DEFAULT.withHeader().parse(br);

        // Initialize the WebDriver and other setup here...

        List<CSVRecord> batch = new ArrayList<>();
        int batchSize = 10;
        int count = 0;

        for (CSVRecord record : records) {
            batch.add(record);
            if (batch.size() == batchSize) {
                processBatch(batch);
                batch.clear(); // Clear the batch after processing
            }
        }

        // Process the last batch if it's not empty
        if (!batch.isEmpty()) {
            processBatch(batch);
        }

        // WebDriver cleanup code here...
    }

    private static void processBatch(List<CSVRecord> batch) throws InterruptedException {
        for (CSVRecord record : batch) {
            WebElement pathInput = driver.findElement(By.xpath("//your_xpath_here"));
            Thread.sleep(1000);
            pathInput.sendKeys(record.get(2));  // Assuming column index 2 has the required data

            WebElement requestedIncrease = driver.findElement(By.xpath("//your_xpath_here"));
            Thread.sleep(2000);
            requestedIncrease.sendKeys("1");

            WebElement addMore = driver.findElement(By.xpath("//your_xpath_for_addMore_button"));
            Thread.sleep(1000);
            addMore.click();

            Thread.sleep(1000);
        }

        // Perform the validation after each batch
        WebElement validateButton = driver.findElement(By.xpath("//your_xpath_for_validate_button"));
        validateButton.click();
        Thread.sleep(2000);  // Adjust timing as necessary
    }
}
