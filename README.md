 @Mock
    private TemplateEngine templateEngine;

    @Mock
    private ContextFactory contextFactory;

    @Mock
    private Context context;


    @Captor
    private ArgumentCaptor<String> stringCaptor;

    @Captor
    private ArgumentCaptor<Object> objectCaptor;

    when(contextFactory.createContext()).thenReturn(context);
        doNothing().when(context).setVariable(anyString(), any());
