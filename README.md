testImplementation 'com.github.tomakehurst:wiremock-jre8:2.32.1'



import com.github.tomakehurst.wiremock.WireMockServer;
import com.github.tomakehurst.wiremock.client.WireMock;
import org.junit.AfterClass;
import org.junit.BeforeClass;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.web.client.RestTemplate;

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
                        .withStatus(404)
                        .withBody("Not Found")));

        RestTemplate restTemplate = new RestTemplate();
        String response = restTemplate.getForObject(apiUrl + "/endpoint", String.class);

        // Add your assertions here
    }

    @Test
    public void testServerErrorResponse() {
        wireMockServer.stubFor(get(urlEqualTo("/endpoint"))
                .willReturn(aResponse()
                        .withStatus(500)
                        .withBody("Internal Server Error")));

        RestTemplate restTemplate = new RestTemplate();
        String response = restTemplate.getForObject(apiUrl + "/endpoint", String.class);

        // Add your assertions here
    }

    @Test
    public void testSuccessfulResponse() {
        wireMockServer.stubFor(get(urlEqualTo("/endpoint"))
                .willReturn(aResponse()
                        .withStatus(200)
                        .withBody("OK")));

        RestTemplate restTemplate = new RestTemplate();
        String response = restTemplate.getForObject(apiUrl + "/endpoint", String.class);

        // Add your assertions here
    }
}
