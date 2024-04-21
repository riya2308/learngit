TemplateEngine templateEngine = mock(TemplateEngine.class);

    String templatePath = "src\\test\\resources\\testFiles\\TestEmailTemplate.html";
    
    when(templateEngine.process(eq(templatePath), any(Context.class)))
        .thenReturn("processed content");
