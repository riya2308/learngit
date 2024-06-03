import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.HashSet;
import java.util.Set;

public class UniqueRecords {
    public static void main(String[] args) {
        String inputFile = "path_to_input_file.xlsx";
        String outputFile = "path_to_output_file.xlsx";

        try (FileInputStream fileInputStream = new FileInputStream(inputFile);
             Workbook workbook = new XSSFWorkbook(fileInputStream);
             Workbook outputWorkbook = new XSSFWorkbook();
             FileOutputStream fileOutputStream = new FileOutputStream(outputFile)) {

            Sheet inputSheet = workbook.getSheetAt(0);
            Sheet outputSheet = outputWorkbook.createSheet("UniqueRecords");

            Set<String> uniqueCombinations = new HashSet<>();
            int outputRowNum = 0;

            for (Row row : inputSheet) {
                Cell groupCell = row.getCell(0); // Assuming 'group' is in the first column
                Cell nameCell = row.getCell(1); // Assuming 'name' is in the second column

                if (groupCell != null && nameCell != null && 
                    groupCell.getCellType() == CellType.STRING && 
                    nameCell.getCellType() == CellType.STRING) {

                    String group = groupCell.getStringCellValue();
                    String name = nameCell.getStringCellValue();

                    if (!group.isEmpty() && !name.isEmpty()) {
                        String combination = group + "+" + name;

                        if (!uniqueCombinations.contains(combination)) {
                            uniqueCombinations.add(combination);

                            // Create a new row in the output sheet and copy values
                            Row outputRow = outputSheet.createRow(outputRowNum++);
                            outputRow.createCell(0).setCellValue(group);
                            outputRow.createCell(1).setCellValue(name);
                        }
                    }
                }
            }

            outputWorkbook.write(fileOutputStream);
            System.out.println("Unique combinations have been written to the output file.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
