public class TestTest {

    @Test
    public void testConstructor_withValidName() {
        // Arrange
        String validName = "John Doe";

        // Act
        Test test = new Test(validName);

        // Assert
        assertNotNull(test);
        assertEquals(validName, test.getName());
    }

    @Test(expected = NullPointerException.class)
    public void testConstructor_withNullName() {
        // Arrange
        String nullName = null;

        // Act
        Test test = new Test(nullName);

        // Expect an exception to be thrown
    }
}
