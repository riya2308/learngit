        when(templateEngine.process(anyString(), any(Context.class))).thenReturn("processedTemplate");
Context context = mock(Context.class);
        whenNew(Context.class).withAnyArguments().thenReturn(context);
import static org.powermock.api.mockito.PowerMockito.whenNew;

import static org.mockito.ArgumentMatchers.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.Context;
