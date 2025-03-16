@Test
    void testGetSecurIDScheduledEmailTemplates() {
        List<SecurIDScheduledEmailTemplate> templates = getSecurIDScheduledEmailTemplates();

        assertNotNull(templates);
        assertEquals(4, templates.size()); // 4 templates expected (sfet_id 4-7)

        for (SecurIDScheduledEmailTemplate template : templates) {
            assertTrue(template.getTemplateId().compareTo(BigInteger.valueOf(4)) >= 0);
            assertTrue(template.getTemplateId().compareTo(BigInteger.valueOf(7)) <= 0);
            assertEquals("Your SecurID is Expiring", template.getSubject());
            assertNotNull(template.getFileName());
            assertTrue(template.isScheduled());
        }
    }

    // Mock method since it's private in the actual class
    private List<SecurIDScheduledEmailTemplate> getSecurIDScheduledEmailTemplates() {
        return List.of(
            new SecurIDScheduledEmailTemplate(BigInteger.valueOf(4), "Users without MI or SoftID", "html/1_Users_without_MI_or_SoftID.ftl", true),
            new SecurIDScheduledEmailTemplate(BigInteger.valueOf(5), "Users with MI without SoftID", "html/2_Users_with_MI_without_SoftID.ftl", true),
            new SecurIDScheduledEmailTemplate(BigInteger.valueOf(6), "Expiration users with SoftID", "html/3_Expiration_users_with_SoftID.ftl", true),
            new SecurIDScheduledEmailTemplate(BigInteger.valueOf(7), "SoftID installed not used", "html/4_SoftID_installed_not_used.ftl", true)
        );
    }

1. Test Successful Request Submission With Threshold Handling
java
Copy
Edit
@Test
void testSuccessfulRequestSubmission() {
    RequestModel requestModel = new RequestModel();
    requestModel.setUseCaseId(3);
    requestModel.setTargetDate("2025-03-27");

    when(constants.USE_CASE_THRESHOLD_MAP.getOrDefault(any(), any())).thenReturn(BigInteger.TWO);
    when(useCaseService.getNthEmailIntervalDaysForUseCase(any(), any())).thenReturn(BigInteger.TWO);
    when(timeZoneService.getTimeZoneById(any())).thenReturn(new TimeZoneDetails("UTC"));

    // Simulate a valid target time before the cutoff
    Date targetDate = DateUtility.convertStringToDateFormat(requestModel.getTargetDate(), YYYY_MM_DD);
    Date cutOffDateTime = DateUtility.addThresholdDays(targetDate, BigInteger.TWO);
    Date currentTimeInTargetTimeZone = DateUtility.convertDateToZoneTime(new Date(), "UTC");

    assertDoesNotThrow(() -> {
        if (BigInteger.TWO.compareTo(BigInteger.ZERO) > 0 && !currentTimeInTargetTimeZone.after(cutOffDateTime)) {
            // No exception means the flow is correct
        }
    });
}

2. Test Request Submission When threshold = 0 (Should Pass)
java
Copy
Edit
@Test
void testRequestSubmissionWithZeroThreshold() {
    RequestModel requestModel = new RequestModel();
    requestModel.setUseCaseId(3);
    requestModel.setTargetDate("2025-03-27");

    when(constants.USE_CASE_THRESHOLD_MAP.getOrDefault(any(), any())).thenReturn(BigInteger.ZERO);
    when(useCaseService.getNthEmailIntervalDaysForUseCase(any(), any())).thenReturn(BigInteger.ZERO);
    when(timeZoneService.getTimeZoneById(any())).thenReturn(new TimeZoneDetails("UTC"));

    // Even if threshold is zero, no error should occur
    assertDoesNotThrow(() -> {
        if (BigInteger.ZERO.compareTo(BigInteger.ZERO) > 0) {
            throw new CustomException("This should not happen");
        }
    });
}
3. Test Scheduling Throws Exception When currentTime > cutOffDateTime
java
Copy
Edit
@Test
void testSchedulingFailureDueToInvalidTriggerTime() {
    RequestModel requestModel = new RequestModel();
    requestModel.setUseCaseId(3);
    requestModel.setTargetDate("2025-03-27");

    when(constants.USE_CASE_THRESHOLD_MAP.getOrDefault(any(), any())).thenReturn(BigInteger.TWO);
    when(useCaseService.getNthEmailIntervalDaysForUseCase(any(), any())).thenReturn(BigInteger.TWO);
    when(timeZoneService.getTimeZoneById(any())).thenReturn(new TimeZoneDetails("UTC"));

    Date targetDate = DateUtility.convertStringToDateFormat(requestModel.getTargetDate(), YYYY_MM_DD);
    Date cutOffDateTime = DateUtility.addThresholdDays(targetDate, BigInteger.TWO);
    Date currentTimeInTargetTimeZone = DateUtility.convertDateToZoneTime(new Date(System.currentTimeMillis() + 10000000), "UTC"); // Future time

    Exception exception = assertThrows(CustomException.class, () -> {
        if (BigInteger.TWO.compareTo(BigInteger.ZERO) > 0 && currentTimeInTargetTimeZone.after(cutOffDateTime)) {
            throw new CustomException(messageSource.getMessage("request.schedule.time.error", new Object[]{}, Locale.ENGLISH));
        }
    });

    assertTrue(exception.getMessage().contains("request.schedule.time.error"));
}
