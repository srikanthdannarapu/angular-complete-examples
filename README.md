import org.springframework.core.env.PropertiesPropertySource;
import org.springframework.core.env.PropertySource;
import org.springframework.core.io.support.DefaultPropertySourceFactory;
import org.springframework.core.io.support.EncodedResource;
import org.springframework.core.io.support.PropertySourceFactory;
import org.yaml.snakeyaml.Yaml;

import java.io.IOException;
import java.util.Properties;

public class YamlPropertySourceFactory extends DefaultPropertySourceFactory implements PropertySourceFactory {

    @Override
    public PropertySource<?> createPropertySource(String name, EncodedResource resource) throws IOException {
        if (resource == null) {
            return super.createPropertySource(name, resource);
        }
        Yaml yaml = new Yaml();
        Object result = yaml.load(resource.getInputStream());
        if (result instanceof Properties) {
            return super.createPropertySource(name, resource);
        } else if (result instanceof Iterable) {
            Properties properties = new Properties();
            int index = 0;
            for (Object object : (Iterable<?>) result) {
                if (object instanceof Properties) {
                    properties.putAll((Properties) object);
                } else {
                    properties.putAll(getPropertiesFromObject(object, index++));
                }
            }
            return new PropertiesPropertySource(name, properties);
        } else {
            return super.createPropertySource(name, resource);
        }
    }

    private Properties getPropertiesFromObject(Object object, int index) {
        Properties properties = new Properties();
        if (object instanceof java.util.LinkedHashMap) {
            java.util.LinkedHashMap<?, ?> map = (java.util.LinkedHashMap<?, ?>) object;
            map.forEach((key, value) -> properties.put("employees[" + index + "]." + key, value));
        }
        return properties;
    }
}
