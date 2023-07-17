@Configuration
@EnableScheduling
public class CryptoConfig {
    private CryptoUtil cryptoUtil;

    @Bean
    public CryptoUtil cryptoUtil() {
        return cryptoUtil;
    }

    @Scheduled(fixedRate = 15000) // Run every 15 seconds
    public void updateCryptoUtil() throws NoSuchAlgorithmException, InvalidKeyException {
        byte[] randomBytes = new byte[512];
        SecureRandom secureRandom = new SecureRandom();
        secureRandom.nextBytes(randomBytes);
        String panHmacKey = new String(randomBytes, StandardCharsets.UTF_8);
        this.cryptoUtil = new CryptoUtil(panHmacKey);
    }
}
