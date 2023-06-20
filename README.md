import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.security.Key;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.KeyStore;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.cert.Certificate;

import javax.crypto.Cipher;

public class KeyPairExample {
    private static final String KEYSTORE_PATH = "/path/to/keystore.jks";
    private static final String TRUSTSTORE_PATH = "/path/to/truststore.jks";
    private static final String KEYSTORE_PASSWORD = "keystore_password";
    private static final String KEY_ALIAS = "key_alias";
    private static final String KEY_PASSWORD = "key_password";

    public static void main(String[] args) throws Exception {
        // Generate a key pair
        KeyPair keyPair = generateKeyPair();

        // Store the keys in the keystore
        storeKeyPair(keyPair);

        // Load the keys from the keystore
        KeyPair loadedKeyPair = loadKeyPair();

        // Encrypt and decrypt a plain text
        String plainText = "Hello, world!";
        String encryptedText = encrypt(plainText, loadedKeyPair.getPublic());
        String decryptedText = decrypt(encryptedText, loadedKeyPair.getPrivate());

        System.out.println("Original text: " + plainText);
        System.out.println("Encrypted text: " + encryptedText);
        System.out.println("Decrypted text: " + decryptedText);
    }

    private static KeyPair generateKeyPair() throws Exception {
        KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        keyPairGenerator.initialize(2048);
        return keyPairGenerator.generateKeyPair();
    }

    private static void storeKeyPair(KeyPair keyPair) throws Exception {
        KeyStore keyStore = KeyStore.getInstance("JKS");
        keyStore.load(null, null);
        keyStore.setKeyEntry(KEY_ALIAS, keyPair.getPrivate(), KEY_PASSWORD.toCharArray(), new Certificate[]{});
        keyStore.store(new FileOutputStream(KEYSTORE_PATH), KEYSTORE_PASSWORD.toCharArray());
    }

    private static KeyPair loadKeyPair() throws Exception {
        KeyStore keyStore = KeyStore.getInstance("JKS");
        keyStore.load(new FileInputStream(KEYSTORE_PATH), KEYSTORE_PASSWORD.toCharArray());
        Key privateKey = keyStore.getKey(KEY_ALIAS, KEY_PASSWORD.toCharArray());
        Certificate cert = keyStore.getCertificate(KEY_ALIAS);
        PublicKey publicKey = cert.getPublicKey();
        return new KeyPair(publicKey, (PrivateKey) privateKey);
    }

    private static String encrypt(String plainText, PublicKey publicKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
        byte[] encryptedBytes = cipher.doFinal(plainText.getBytes());
        return new String(encryptedBytes);
    }

    private static String decrypt(String encryptedText, PrivateKey privateKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        byte[] decryptedBytes = cipher.doFinal(encryptedText.getBytes());
        return new String(decryptedBytes);
    }
}



keytool -genkeypair -alias mykey -keyalg RSA -keysize 2048 -keystore keystore.jks


keytool -export -alias mykey -file mycert.cer -keystore keystore.jks
keytool -import -alias mykey -file mycert.cer -keystore truststore.jks



keytool -export -alias mykey -file mycert.cer -keystore keystore.jks
