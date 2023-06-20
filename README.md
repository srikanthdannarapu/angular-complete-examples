import com.github.tomakehurst.wiremock.WireMockServer;
import com.github.tomakehurst.wiremock.client.WireMock;
import org.junit.AfterClass;
import org.junit.BeforeClass;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.core.ParameterizedTypeReference;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.web.client.RestTemplate;

import java.util.List;

import static com.github.tomakehurst.wiremock.client.WireMock.*;

@RunWith(SpringRunner.class)
@SpringBootTest
public class RestTemplateExampleTest {

    private static WireMockServer wireMockServer;

    @Value("${api.base.url}")
    private String apiUrl;

    @BeforeClass
    public static void setup() {
        wireMockServer = new WireMockServer();
        wireMockServer.start();
        WireMock.configureFor("localhost", wireMockServer.port());
    }

    @AfterClass
    public static void teardown() {
        wireMockServer.stop();
    }

    @Test
    public void testErrorResponse() {
        wireMockServer.stubFor(get(urlEqualTo("/endpoint"))
                .willReturn(aResponse()
                        .withStatus(404)));

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.getForEntity(apiUrl + "/endpoint", String.class);

        // Add your assertions here
        // Verify the response status code is 404
    }

    @Test
    public void testServerErrorResponse() {
        wireMockServer.stubFor(get(urlEqualTo("/endpoint"))
                .willReturn(aResponse()
                        .withStatus(500)));

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<String> response = restTemplate.getForEntity(apiUrl + "/endpoint", String.class);

        // Add your assertions here
        // Verify the response status code is 500
    }

    @Test
    public void testSuccessfulResponse() {
        List<Employee> mockEmployees = List.of(
                new Employee(1, "John Doe"),
                new Employee(2, "Jane Smith")
        );

        wireMockServer.stubFor(get(urlEqualTo("/endpoint"))
                .willReturn(aResponse()
                        .withStatus(200)
                        .withHeader("Content-Type", "application/json")
                        .withBody(objectMapper.writeValueAsString(mockEmployees))));

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<List<Employee>> response = restTemplate.exchange(
                apiUrl + "/endpoint",
                HttpMethod.GET,
                null,
                new ParameterizedTypeReference<List<Employee>>() {});

        List<Employee> employees = response.getBody();

        // Add your assertions here
        // Verify the content of the 'employees' list
    }
}
