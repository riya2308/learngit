import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;

public class FormAutomation {
    public static void main(String[] args) {
        System.setProperty("webdriver.chrome.driver", "path_to_chromedriver");

        WebDriver driver = new ChromeDriver();
        driver.get("http://voltaire.webfarm.ms.com/requestor/create");  // Replace with your actual URL

        try {
            // Ensure the page is fully loaded
            Thread.sleep(10000);  // Wait for 10 seconds

            // Send 12 TAB keys to navigate through the form
            WebElement body = driver.findElement(By.tagName("body"));
            for (int i = 0; i < 12; i++) {
                body.sendKeys(Keys.TAB);
                Thread.sleep(300);  // Wait for 300 milliseconds after each TAB
            }

            // Select a region from a dropdown
            Select regionDropdown = new Select(driver.findElement(By.id("regionDropdownId")));  // Replace 'regionDropdownId' with actual ID
            regionDropdown.selectByVisibleText("Asia Pacific");  // Replace 'Asia Pacific' with the actual option you need

            // Send additional TAB keystroke(s) to move to the next field
            body.sendKeys(Keys.TAB);
            Thread.sleep(300);  // Adjust based on how form reacts, may need more TABs

            // Select a type from another dropdown
            Select typeDropdown = new Select(driver.findElement(By.id("typeDropdownId")));  // Replace 'typeDropdownId' with actual ID
            typeDropdown.selectByVisibleText("Type1");  // Replace 'Type1' with the actual option you need

            // Send additional TAB keystroke to move to the next field
            body.sendKeys(Keys.TAB);
            Thread.sleep(300);  // Wait for 300 milliseconds

            // Select a form from another dropdown
            Select formDropdown = new Select(driver.findElement(By.id("formDropdownId")));  // Replace 'formDropdownId' with actual ID
            formDropdown.selectByVisibleText("Form1");  // Replace 'Form1' with the actual option you need

            // Optionally, submit the form if needed
            // driver.findElement(By.id("submitBtnId")).click();  // Replace 'submitBtnId' with actual ID

        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            driver.quit();  // Close the browser
        }
    }
}





//by id
WebElement asiaRadioButton = driver.findElement(By.id("asia"));
            asiaRadioButton.click();
            Thread.sleep(500); 
