        when(templateEngine.process(anyString(), any(Context.class))).thenReturn("processedTemplate");
Context context = mock(Context.class);
        whenNew(Context.class).withAnyArguments().thenReturn(context);
