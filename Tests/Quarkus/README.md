# Quarkus Tests

## Unit Tests
Unit tests in Quarkus are similar to standard JUnit tests. In this example, we'll create a simple service and test it.

```
@ApplicationScoped
public class GreetingService {

    public String greet(String name) {
        return "Hello, " + name + "!";
    }
}
```
Now, let's write a unit test for the GreetingService using JUnit 5:

```
import io.quarkus.test.junit.QuarkusTest;
import org.junit.jupiter.api.Test;

import javax.inject.Inject;

import static org.junit.jupiter.api.Assertions.assertEquals;

@QuarkusTest
public class GreetingServiceTest {

    @Inject
    GreetingService greetingService;

    @Test
    public void testGreet() {
        String greeting = greetingService.greet("John");
        assertEquals("Hello, John!", greeting);
    }
}
```

In this unit test, we use the @QuarkusTest annotation to enable Quarkus testing support. We inject the GreetingService bean and test its functionality.

## Integration Tests
Integration tests in Quarkus allow you to test your application in a more real-world scenario. In this example, let's assume we have a RESTful resource that we want to test.

```
@Path("/api/greetings")
public class GreetingResource {

    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String getGreeting() {
        return "Hello, world!";
    }
}
```

Here's an integration test for the GreetingResource:

```
import io.quarkus.test.junit.QuarkusTest;
import io.restassured.RestAssured;
import org.junit.jupiter.api.Test;

import static io.restassured.RestAssured.given;
import static org.hamcrest.Matchers.is;

@QuarkusTest
public class GreetingResourceIT {

    @Test
    public void testGreetingEndpoint() {
        given()
            .when().get("/api/greetings")
            .then()
            .statusCode(200)
            .body(is("Hello, world!"));
    }
}
```

In this integration test, we use the @QuarkusTest annotation to start a real server. We then use RestAssured, a popular testing framework for RESTful services, to send an HTTP request to the /api/greetings endpoint and validate the response.

These examples demonstrate how to write unit tests and integration tests in a Quarkus project. Proper testing ensures the reliability and functionality of your Quarkus applications in different scenarios, from individual components to interactions with external resources like RESTful APIs.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)

