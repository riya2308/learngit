import java.util.List;
import java.util.Map;

public void updateMailStatus(Map<Integer, List<GsData>> mailGroupDataHashMap) {
    for (Map.Entry<Integer, List<GsData>> entry : mailGroupDataHashMap.entrySet()) {
        Integer key = entry.getKey();
        List<GsData> dataList = entry.getValue();

        // Update logic based on your requirement
        boolean mailSent = determineIfMailSent(dataList); // Implement this method based on your logic
        int newCount = countNewRecords(dataList); // Implement this method to count new records
        int totalCount = dataList.size();

        // Log the update for debugging
        System.out.println("Updating mail group " + key + ": Mail Sent = " + mailSent + ", New Count = " + newCount + ", Total Count = " + totalCount);

        // Update the status in your database or another structure here
        updateDatabaseOrStructure(key, mailSent, newCount, totalCount);  // Define this method as per your requirement
    }
}

private boolean determineIfMailSent(List<GsData> dataList) {
    // Define logic to determine if mail was sent
    return true; // Placeholder
}

private int countNewRecords(List<GsData> dataList) {
    // Define logic to count new records
    int count = 0;
    for (GsData data : dataList) {
        if (data.isNewRecord()) {  // Assuming GsData has an isNewRecord() method to check if the record is new
            count++;
        }
    }
    return count;
}

private void updateDatabaseOrStructure(Integer key, boolean mailSent, int newCount, int totalCount) {
    // Update the database or any other structure you are using to track these statuses
    System.out.println("Database updated for key: " + key);  // Placeholder
}
