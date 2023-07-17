@Configuration
@EnableScheduling
public class CryptoConfig {
    private String panHmacKey;

    @Bean
    public CryptoUtil cryptoUtil() throws NoSuchAlgorithmException, InvalidKeyException {
        return new CryptoUtil(panHmacKey);
    }

    @Scheduled(fixedRate = 24 * 60 * 60 * 1000) // Run every 24 hours
    public void generatePanHmacKey() {
        byte[] randomBytes = new byte[512];
        SecureRandom secureRandom = new SecureRandom();
        secureRandom.nextBytes(randomBytes);
        panHmacKey = new String(randomBytes, StandardCharsets.UTF_8);
    }
}
