import org.junit.Before;
import org.junit.Test;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.MockitoAnnotations;

import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;

import static org.junit.Assert.assertEquals;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;

public class CryptoConfigTest {

    private CryptoConfig cryptoConfig;

    @Mock
    private MyService myService;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        cryptoConfig = new CryptoConfig();
        cryptoConfig.setMyService(myService);
    }

    @Test
    public void testUpdateCryptoUtil() throws NoSuchAlgorithmException, InvalidKeyException {
        // Mocking SecureRandom and generating a test panHmacKey
        SecureRandom secureRandomMock = Mockito.mock(SecureRandom.class);
        byte[] randomBytes = new byte[512];
        when(secureRandomMock.nextBytes(Mockito.any(byte[].class))).then(invocation -> {
            byte[] bytesArg = invocation.getArgument(0);
            System.arraycopy(randomBytes, 0, bytesArg, 0, bytesArg.length);
            return null;
        });

        // Mocking the MyService dependency
        CryptoUtil cryptoUtilMock = Mockito.mock(CryptoUtil.class);
        when(myService.getCryptoUtil()).thenReturn(cryptoUtilMock);

        // Invoking the updateCryptoUtil method
        cryptoConfig.updateCryptoUtil();

        // Verifying that the panHmacKey is set and passed to the MyService dependency
        verify(myService).setCryptoUtil(Mockito.any(CryptoUtil.class));
        assertEquals(new String(randomBytes, StandardCharsets.UTF_8), cryptoConfig.getCryptoUtil().getPanHmacKey());
    }
}
