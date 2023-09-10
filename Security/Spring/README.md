# Securing Your Spring Boot API

Securing your Spring Boot API is a critical aspect of building a robust and reliable application, especially when dealing with sensitive data and user authentication. In this guide, we'll explore how to configure security for your Spring Boot API using Spring Security, along with practical examples.

## 1. Adding Spring Security to Your Project

To get started with securing your Spring Boot API, you need to include Spring Security in your project's dependencies. In your build.gradle (for Gradle) or pom.xml (for Maven), add the following dependency:

```
<!-- For Gradle -->
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-security'
}
```

```
<!-- For Maven -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

## 2. Basic Authentication Configuration
Example 1: Basic Authentication
To enable basic authentication for your API, you can use Spring Security's default settings. Create a configuration class that extends WebSecurityConfigurerAdapter:

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public").permitAll()
                .anyRequest().authenticated()
                .and()
            .httpBasic();
    }

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
            .username("user")
            .password("password")
            .roles("USER")
            .build();
        
        return new InMemoryUserDetailsManager(user);
    }
}
```

In this example, we have configured basic authentication, allowing access to the "/public" endpoint without authentication, and requiring authentication for all other requests.

## 3. Token-Based Authentication with JWT
Example 2: JWT Authentication
To implement token-based authentication using JSON Web Tokens (JWT), you can follow these steps:

Include the necessary dependencies for JWT and create JWT-related classes.
Configure Spring Security to use JWT authentication.
Here's a simplified example:

```
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public").permitAll()
                .anyRequest().authenticated()
                .and()
            .addFilterBefore(new JwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);
    }
}
```

In this example, we configure Spring Security to use JWT authentication by adding a custom JwtAuthenticationFilter before the default UsernamePasswordAuthenticationFilter.

## 4. OAuth2 Authentication
Example 3: OAuth2 Authentication
For OAuth2 authentication, you can use Spring Security's OAuth2 support. Here's an example of configuring OAuth2 with Google as the identity provider:

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public").permitAll()
                .anyRequest().authenticated()
                .and()
            .oauth2Login()
                .loginPage("/login")
                .defaultSuccessURL("/home");
    }
}
```

In this example, we configure OAuth2 login and specify Google as the identity provider. The loginPage and defaultSuccessURL are customized to handle authentication and redirection.

If you want to use users stored in a database for authentication in your Spring Boot API, you can do so by integrating Spring Security with a database-backed user authentication system. Here's a step-by-step guide with code examples on how to achieve this:

## 5. Configure Database for User Storage
First, you need to have a database where user information is stored. Commonly, this database contains tables for users, roles, and their associations.

Here's an example of a User entity class representing users:

```
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
    private String password;
    // Other user details, roles, etc.
    
    // Getters and setters
}
```

## 6. Create a Custom UserDetailsService
You will need to create a custom UserDetailsService that Spring Security can use to load user details from the database. Here's an example:

```
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found with username: " + username));

        return new org.springframework.security.core.userdetails.User(
            user.getUsername(), 
            user.getPassword(), 
            getAuthorities(user.getRoles())
        );
    }

    private Set<GrantedAuthority> getAuthorities(Set<Role> roles) {
        Set<GrantedAuthority> authorities = new HashSet<>();
        for (Role role : roles) {
            authorities.add(new SimpleGrantedAuthority(role.getName()));
        }
        return authorities;
    }
}
```

In this example, CustomUserDetailsService loads user details from the database using the UserRepository and maps them to Spring Security's UserDetails interface.

## 7. Configure AuthenticationManager
Next, configure the AuthenticationManager to use your custom UserDetailsService. You can do this by extending WebSecurityConfigurerAdapter:

```
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private CustomUserDetailsService customUserDetailsService;

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(customUserDetailsService);
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // Configure HTTP security as needed
    }
}
```

In this configuration class, we configure Spring Security to use our custom UserDetailsService.

## 8. Implement Authentication and Authorization
Now, you can secure your API endpoints using authentication and authorization based on user roles. You can use annotations like @PreAuthorize or @Secured on your controller methods to specify access control.

```
@RestController
@RequestMapping("/api")
public class MyController {

    @PreAuthorize("hasRole('USER')")
    @GetMapping("/user")
    public ResponseEntity<String> userEndpoint() {
        // Your user-specific endpoint logic here
    }
    
    @PreAuthorize("hasRole('ADMIN')")
    @GetMapping("/admin")
    public ResponseEntity<String> adminEndpoint() {
        // Your admin-specific endpoint logic here
    }
}
```

In this example, the @PreAuthorize annotation is used to specify that only users with the role 'USER' can access the /api/user endpoint, and only users with the role 'ADMIN' can access the /api/admin endpoint.

With these steps, you can use users stored in a database for authentication and implement role-based access control in your Spring Boot API.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
