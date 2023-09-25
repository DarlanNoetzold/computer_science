# Example with Quarkus

## 1. Dependency Injection:
Explanation: Dependency Injection is a design pattern where components' dependencies are provided to them rather than the components creating their dependencies themselves. In Quarkus, this is achieved through constructor injection.

```
import javax.enterprise.context.ApplicationScoped;
import javax.inject.Inject;

@ApplicationScoped
public class MyService {
    private final MyRepository repository;

    @Inject
    public MyService(MyRepository repository) {
        this.repository = repository;
    }

    // ...
}
```

In this example, MyService receives an instance of MyRepository via constructor-based dependency injection.

## 2. MVC (Model-View-Controller):
Explanation: The Model-View-Controller (MVC) pattern is used to separate an application into three interconnected components: Model, View, and Controller. In Quarkus, JAX-RS is often used for building RESTful APIs following this pattern.

Controller:

```

import javax.inject.Inject;
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import java.util.List;

@Path("/products")
public class ProductResource {
    @Inject
    private ProductService productService;

    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public List<Product> listProducts() {
        return productService.getAllProducts();
    }

    // ...
}
```

In this example, the ProductResource acts as the controller, handling HTTP requests, and using the ProductService to retrieve and return data.

## 3. Singleton:
Explanation: Quarkus, like most modern frameworks, manages beans as singletons by default. There's no specific code required to make a bean a singleton in Quarkus.

```
@ApplicationScoped
public class MySingletonBean {
    // ...
}
```

Here, MySingletonBean is an application-scoped bean, which is effectively a singleton in a Quarkus application.

## 4. Factory Method:
Explanation: Factory methods can still be used in Quarkus to define beans, typically within @ApplicationScoped or @RequestScoped beans.

```
@ApplicationScoped
public class MyBeanFactory {
    public MyBean createMyBean() {
        return new MyBean();
    }
}
```

## 5. AOP (Aspect-Oriented Programming):
Explanation: Quarkus doesn't have built-in AOP support like Spring AOP. However, you can achieve similar functionality using libraries like CDI Interceptors.

```
import javax.interceptor.AroundInvoke;
import javax.interceptor.Interceptor;
import javax.interceptor.InvocationContext;

@Interceptor
public class LoggingInterceptor {
    @AroundInvoke
    public Object logMethodCall(InvocationContext context) throws Exception {
        // Logging logic before method invocation
        Object result = context.proceed();
        // Logging logic after method invocation
        return result;
    }
}
```

In this example, the LoggingInterceptor intercepts method calls and can perform logging before and after the method invocation.

## 6. DAO (Data Access Object):
Explanation: The DAO pattern can be used in Quarkus similar to how it's used in other Java applications to separate data access logic from the rest of the application.

```
import io.quarkus.hibernate.orm.panache.PanacheRepository;

@ApplicationScoped
public class UserDao implements PanacheRepository<User> {
    public User getUserById(Long userId) {
        return findById(userId);
    }

    // ...
}
```

In this example, UserDao extends PanacheRepository to handle data access for the User entity.

## 7. Strategy:
Explanation: The Strategy pattern in Quarkus is similar to other Java applications and can be used to choose between different implementations of a behavior.

```
@ApplicationScoped
public class AuthenticationStrategyFactory {
    @Inject
    @ConfigProperty(name = "auth.strategy")
    String strategy;

    public AuthenticationStrategy getAuthenticationStrategy() {
        if ("username-password".equals(strategy)) {
            return new UsernamePasswordAuthStrategy();
        } else if ("oauth".equals(strategy)) {
            return new OAuthAuthStrategy();
        }
        throw new IllegalArgumentException("Invalid authentication strategy");
    }
}
```

In this example, the AuthenticationStrategyFactory determines the appropriate authentication strategy based on a configuration property and provides the corresponding implementation.

These are explanations and code examples for the mentioned design patterns in a Quarkus context. Similar to Spring, these patterns are fundamental in building modular and maintainable software using Quarkus.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
