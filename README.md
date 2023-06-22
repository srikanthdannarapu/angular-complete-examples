import org.junit.Test;

import javax.validation.*;
import javax.validation.constraints.NotNull;
import java.util.Set;

import static org.junit.Assert.*;

public class TestTest {

    private static ValidatorFactory validatorFactory = Validation.buildDefaultValidatorFactory();
    private static Validator validator = validatorFactory.getValidator();

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

    @Test
    public void testConstructor_withNullName() {
        // Arrange
        String nullName = null;

        // Act
        Set<ConstraintViolation<Test>> violations = validator.validateValue(Test.class, "name", nullName);

        // Assert
        assertFalse(violations.isEmpty());
        assertEquals(1, violations.size());
        assertEquals("may not be null", violations.iterator().next().getMessage());
    }
}
