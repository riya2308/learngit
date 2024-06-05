import static org.mockito.Mockito.*;
import static org.junit.Assert.*;
import org.apache.poi.xssf.usermodel.*;
import org.junit.*;
import java.util.*;

public class WriteForKeyTest {
    private static final String SHEET_NAME = "TestSheet";

    @Test
    public void testSuccessfulExecution() {
        // Prepare data
        Integer keyValue = 10;
        List<GsData> rows = Arrays.asList(new GsData(), new GsData()); // Assume GsData is a valid class

        // Mocking Excel components
        XSSFWorkbook mockedWorkbook = mock(XSSFWorkbook.class);
        XSSFSheet mockedSheet = mock(XSSFSheet.class);
        XSSFRow mockedRow = mock(XSSFRow.class);
        XSSFCellStyle mockedCellStyle = mock(XSSFCellStyle.class);

        when(mockedWorkbook.createSheet(SHEET_NAME)).thenReturn(mockedSheet);
        when(mockedSheet.createRow(anyInt())).thenReturn(mockedRow);
        when(mockedWorkbook.createCellStyle()).thenReturn(mockedCellStyle);

        // Call the method under test
        YourClass instance = new YourClass(); // YourClass should be replaced with the actual class name
        instance.writeForKey(keyValue, rows);

        // Verify the interactions
        verify(mockedSheet, times(rows.size() + 1)).createRow(anyInt()); // +1 for the header row
        verify(mockedCellStyle, times(1)).setBorderTop(BorderStyle.THIN);
        verify(mockedCellStyle, times(1)).setBorderBottom(BorderStyle.THIN);
        verify(mockedCellStyle, times(1)).setBorderLeft(BorderStyle.THIN);
        verify(mockedCellStyle, times(1)).setBorderRight(BorderStyle.THIN);
        verify(mockedSheet, times(1)).setDefaultColumnStyle(anyInt(), eq(mockedCellStyle));
    }

    @Test
    public void testExceptionHandling() {
        Integer keyValue = 10;
        List<GsData> rows = Arrays.asList(new GsData());

        // Mocking to throw an exception
        XSSFWorkbook mockedWorkbook = mock(XSSFWorkbook.class);
        when(mockedWorkbook.createSheet(SHEET_NAME)).thenThrow(new RuntimeException("Test exception"));

        // Setup the class under test
        YourClass instance = new YourClass();
        try {
            instance.writeForKey(keyValue, rows);
            fail("Expected an exception to be thrown");
        } catch (RuntimeException e) {
            assertEquals("Test exception", e.getMessage());
        }

        // Ensuring the mail method is called on failure
        // This assumes sendMailOnFailure is accessible or observable through some means (e.g., another mock or a spy)
    }
}
