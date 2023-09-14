# Tests in Spring

## Unit Tests
Unit tests focus on testing individual components or classes in isolation. In this example, we have a simple Calculator class that we want to test.

```
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```
Now, let's write a JUnit test for the Calculator class:
```
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {

    @Test
    public void testAdd() {
        Calculator calculator = new Calculator();
        int result = calculator.add(2, 3);
        assertEquals(5, result);
    }
}
```

In this unit test, we create an instance of Calculator, call the add method with two numbers, and assert that the result is equal to the expected value.

## Integration Tests
Integration tests verify the interactions between different components or modules within your application. In this example, let's assume we have a service called UserService that interacts with a database.

```
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public User createUser(User user) {
        return userRepository.save(user);
    }
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // Custom queries can go here
}
```

Here's an integration test for the UserService:

```
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.mock.mockito.MockBean;

import static org.mockito.Mockito.*;

@SpringBootTest
public class UserServiceIntegrationTest {

    @Autowired
    private UserService userService;

    @MockBean
    private UserRepository userRepository;

    @Test
    public void testCreateUser() {
        User user = new User("John Doe");
        when(userRepository.save(user)).thenReturn(user);

        User createdUser = userService.createUser(user);

        verify(userRepository, times(1)).save(user);
        assertEquals(user, createdUser);
    }
}
```

In this integration test, we use @SpringBootTest to load the Spring application context, and we mock the UserRepository using @MockBean. We then verify that the UserService interacts correctly with the repository.

## End-to-End Tests
End-to-end tests, often used for web applications, simulate user interactions. For simplicity, let's assume we have a RESTful API endpoint for creating users.

```
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return new ResponseEntity<>(createdUser, HttpStatus.CREATED);
    }
}
```

Here's an end-to-end test using Spring's TestRestTemplate to test the /api/users endpoint:

```
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.ResponseEntity;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class UserControllerEndToEndTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void testCreateUser() {
        User user = new User("Jane Smith");
        ResponseEntity<User> response = restTemplate.postForEntity("/api/users", user, User.class);

        assertEquals(HttpStatus.CREATED, response.getStatusCode());
        assertNotNull(response.getBody());
        assertEquals(user.getName(), response.getBody().getName());
    }
}
```
In this end-to-end test, we use @SpringBootTest with webEnvironment set to RANDOM_PORT to start a real server. We then use TestRestTemplate to make HTTP requests to the endpoint and validate the response.

These examples demonstrate how to write unit tests, integration tests, and end-to-end tests in a Spring project. Proper testing ensures that your application functions correctly and reliably in different scenarios, from individual components to interactions between different parts of your application.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
