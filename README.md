String templatePath = "src\\\\test\\\\resources\\\\testFiles\\\\TestEmailTemplate.html";
    Mockito.when(templateEngine.process(
        Mockito.eq(templatePath), 
        Mockito.any())
    ).thenReturn("processed content");
