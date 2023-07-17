import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.MockitoAnnotations;
import org.springframework.scheduling.annotation.Scheduled;

import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CryptoConfigTest {

    private CryptoConfig cryptoConfig;

    @Mock
    private MyService myService;

    @BeforeEach
    public void setup() {
        MockitoAnnotations.openMocks(this);
        cryptoConfig = new CryptoConfig();
        cryptoConfig.setMyService(myService);
    }

    @Test
    public void testUpdateCryptoUtil() throws NoSuchAlgorithmException, InvalidKeyException {
        // Mocking SecureRandom and generating a test panHmacKey
        SecureRandom secureRandomMock = Mockito.mock(SecureRandom.class);
        byte[] randomBytes = new byte[512];
        secureRandomMock.nextBytes(randomBytes);

        // Mocking the MyService dependency
        CryptoUtil cryptoUtilMock = Mockito.mock(CryptoUtil.class);
        Mockito.when(myService.getCryptoUtil()).thenReturn(cryptoUtilMock);

        // Invoking the updateCryptoUtil method
        cryptoConfig.updateCryptoUtil();

        // Verifying that the panHmacKey is set and passed to the MyService dependency
        Mockito.verify(myService).setCryptoUtil(Mockito.any(CryptoUtil.class));
        assertEquals(new String(randomBytes, StandardCharsets.UTF_8), cryptoConfig.getCryptoUtil().getPanHmacKey());
    }
}
