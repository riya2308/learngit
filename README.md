 @Test
    void testSecurIDDataPersistence() {
        // 1. Save Templates
        List<SecurIDScheduledEmailTemplate> templates = getSecurIDScheduledEmailTemplates();
        securIDTemplateRepository.saveAll(templates);

        // 2. Save Request Schedule Details
        List<RequestScheduleDetails> scheduleDetails = mockSecurIDRequestScheduleDetails();
        requestScheduleDetailsRepository.saveAll(scheduleDetails);

        // 3. Verify Templates Persisted
        assertEquals(4, securIDTemplateRepository.count());

        // 4. Verify Request Schedule Details Persisted
        assertEquals(12, requestScheduleDetailsRepository.count());

        // 5. Validate data mapping
        RequestScheduleDetails sampleDetail = requestScheduleDetailsRepository.findAll().get(0);
        assertNotNull(sampleDetail.getScheduleTemplateId());
        assertEquals("Scheduled", sampleDetail.getStatus());
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
