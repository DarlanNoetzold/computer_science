# Examples with Spring

## 1. Dependency Injection:
Explanation: Dependency Injection is a design pattern where the components' dependencies are provided to them rather than the components creating their dependencies themselves. In Spring, this is achieved through Inversion of Control (IoC) and is commonly done using annotations like @Autowired.

```
@Service
public class MyService {
    private final MyRepository repository;

    @Autowired
    public MyService(MyRepository repository) {
        this.repository = repository;
    }

    // ...
}
```
In this example, MyService receives an instance of MyRepository via constructor-based dependency injection.

## 2. MVC (Model-View-Controller):
Explanation: The Model-View-Controller (MVC) pattern is used to separate an application into three interconnected components: Model, View, and Controller. In Spring MVC, controllers handle HTTP requests, models represent data, and views render the data.

Controller:

```
@Controller
@RequestMapping("/products")
public class ProductController {
    @Autowired
    private ProductService productService;

    @GetMapping
    public String listProducts(Model model) {
        List<Product> products = productService.getAllProducts();
        model.addAttribute("products", products);
        return "product/list";
    }

    // ...
}
```
In this example, the ProductController handles requests for listing products, and it uses the ProductService to retrieve the data and populate the model.

## 3. Singleton:
Explanation: Singleton is a creational design pattern that ensures a class has only one instance and provides a global point of access to it. In Spring, beans are singletons by default when managed by the Spring container.

```
@Component
public class MySingletonBean {
    // ...
}
```
Here, MySingletonBean is a Spring-managed singleton bean.

## 4. Factory Method:
Explanation: The Factory Method pattern defines an interface for creating an object but lets subclasses alter the type of objects that will be created. In Spring, factory methods are often used in @Configuration classes to define beans.

```
@Configuration
public class MyConfiguration {
    @Bean
    public MyBean createMyBean() {
        return new MyBean();
    }
}
```
In this example, the createMyBean method is a factory method that creates and configures a MyBean instance as a Spring bean.

## 5. AOP (Aspect-Oriented Programming):
Explanation: Aspect-Oriented Programming (AOP) is a programming paradigm that allows for modularizing cross-cutting concerns (e.g., logging, security) separately from the main application logic. Spring AOP provides a way to define aspects and apply them to various parts of the application.

```
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.myapp.service.*.*(..))")
    public void logBeforeMethod(JoinPoint joinPoint) {
        // Logging logic
    }
}
```
In this example, the LoggingAspect aspect logs information before methods execution in classes within the com.example.myapp.service package.

## 6. DAO (Data Access Object):
Explanation: The Data Access Object (DAO) pattern separates the data access logic from the rest of the application. It typically includes methods for CRUD (Create, Read, Update, Delete) operations on data sources.

```
@Repository
public class UserDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public User getUserById(Long userId) {
        String sql = "SELECT * FROM users WHERE id = ?";
        return jdbcTemplate.queryForObject(sql, new Object[]{userId}, new UserRowMapper());
    }

    // ...
}
```
Here, UserDao is a Spring-managed DAO that handles data access logic for the User entity.

## 7. Strategy:
Explanation: The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. In Spring, this pattern is often used for choosing between different implementations of a behavior.

```
public interface AuthenticationStrategy {
    boolean authenticate(User user);
}

@Component
public class UsernamePasswordAuthStrategy implements AuthenticationStrategy {
    @Override
    public boolean authenticate(User user) {
        // Username-password authentication implementation
    }
}

@Component
public class OAuthAuthStrategy implements AuthenticationStrategy {
    @Override
    public boolean authenticate(User user) {
        // OAuth authentication implementation
    }
}
```
In this example, two authentication strategies (UsernamePasswordAuthStrategy and OAuthAuthStrategy) implement the AuthenticationStrategy interface, allowing for interchangeable authentication methods.

These are explanations and code examples for the mentioned design patterns in a Spring context. These patterns are fundamental in building maintainable and modular software using the Spring Framework.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
