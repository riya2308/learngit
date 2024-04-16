# learngit
learning git
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

class ExcelUtilTest {
  
  @Test
  void testCopyHeaderRow() {
    Object[] src = {"Header1", "Header2", null, "Header3"};
    XSSFRow dest = Mockito.mock(XSSFRow.class);
    ExcelUtil util = new ExcelUtil(); // Assume this is your utility class containing copyHeaderRow
    
    util.copyHeaderRow(src, dest);

    Mockito.verify(dest).createCell(0).setCellValue("Header1");
    Mockito.verify(dest).createCell(1).setCellValue("Header2");
    Mockito.verify(dest, Mockito.never()).createCell(2); // as src[2] is null
    Mockito.verify(dest).createCell(3).setCellValue("Header3");
  }
}


import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

class GroupingUtilTest {
  
  @Test
  void testGroupByMailGroup() {
    List<GsData> dataList = Arrays.asList(mock(GsData.class), mock(GsData.class));
    Map<Integer, List<GsData>> expectedMap = new HashMap<>();
    expectedMap.put(1, dataList);

    Constants constantsMock = Mockito.mock(Constants.class);
    when(constantsMock.findList()).thenReturn(dataList);

    GroupingUtil util = new GroupingUtil(constantsMock); // Assume GroupingUtil is the class containing groupByMailGroup
    
    util.groupByMailGroup();

    // Since the actual logic is not visible, here we can only check for the expected result
    assertEquals(expectedMap, util.mailGroupDataHashMap); // Assuming mailGroupDataHashMap is accessible
  }
}

import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import java.io.FileOutputStream;

class WriterUtilTest {
  
  @Test
  void testWriteForKey() {
    Integer keyValue = 1;
    List<GsData> rows = Arrays.asList(mock(GsData.class), mock(GsData.class));
    Object[] headerRow = {"Header1", "Header2", "Header3"};
    XSSFWorkbook workbookMock = Mockito.mock(XSSFWorkbook.class);
    XSSFRow rowMock = Mockito.mock(XSSFRow.class);
    FileOutputStream fileOutputStreamMock = Mockito.mock(FileOutputStream.class);

    WriterUtil util = new WriterUtil(); // Assume this is the class containing writeForKey
    util.setWorkbook(workbookMock); // Assuming there's a method to set the workbook
    util.setFileOutputStream(fileOutputStreamMock); // Assuming there's a method to set the file output stream
    
    util.writeForKey(keyValue, rows, headerRow);

    Mockito.verify(workbookMock).createSheet(any(String.class));
    Mockito.verify(rowMock, Mockito.times(rows.size() + 1)).createCell(anyInt());
    Mockito.verify(fileOutputStreamMock).write(any(byte[].class));
    Mockito.verify(fileOutputStreamMock).close();
  }
}
