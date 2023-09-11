# Securing Your Quarkus

## 1. Configure Database for User Storage

First, ensure that you have a database where user information is stored. Typically, you'll have tables for users, roles, and their associations. Here's an example of a User entity class representing users:

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

## 2. Create a Custom Identity Store
In Quarkus, you can create a custom identity store to interact with your database for user authentication. Implement the IdentityStore interface and provide the logic to load users from your database.

```
@ApplicationScoped
public class CustomIdentityStore implements IdentityStore {

    @Inject
    private EntityManager em;

    @Override
    public CredentialValidationResult validate(Credential credential) {
        if (credential instanceof UsernamePasswordCredential) {
            UsernamePasswordCredential upc = (UsernamePasswordCredential) credential;
            String username = upc.getCaller();
            String password = upc.getPasswordAsString();

            User user = em.createQuery("SELECT u FROM User u WHERE u.username = :username", User.class)
                    .setParameter("username", username)
                    .getResultList()
                    .stream()
                    .findFirst()
                    .orElse(null);

            if (user != null && passwordMatches(user, password)) {
                return new CredentialValidationResult(username, new HashSet<>(user.getRoles()));
            }
        }

        return CredentialValidationResult.INVALID_RESULT;
    }

    private boolean passwordMatches(User user, String password) {
        // Implement your password hashing and matching logic here
        // Compare the hashed password in the user entity with the provided password
        return user.getPassword().equals(password);
    }
}
```
In this example, we've created a CustomIdentityStore class that implements the IdentityStore interface. It validates the user's credentials by querying the database for the user and comparing the hashed password.

## 3. Configure Security in Application Properties
In your application.properties or application.yaml file, configure the security settings and specify your custom identity store:

```
quarkus.security.jdbc.realm-name=myRealm
quarkus.security.jdbc.principal-query=SELECT password FROM User WHERE username=?
quarkus.security.jdbc.principal-query-parameter-index=1
quarkus.security.jdbc.password-query=SELECT password FROM User WHERE username=?
quarkus.security.jdbc.password-query-parameter-index=1
```

These properties configure your custom identity store with Quarkus and specify how to perform principal (username) and password queries against your database.

## 4. Secure Endpoints
You can secure your API endpoints using role-based access control by annotating your JAX-RS resources or methods with @RolesAllowed:

```
@Path("/api")
@RolesAllowed("USER")
public class MyResource {

    @GET
    @Path("/user")
    public String userEndpoint() {
        // Your user-specific endpoint logic here
        return "User endpoint";
    }

    @GET
    @Path("/admin")
    @RolesAllowed("ADMIN")
    public String adminEndpoint() {
        // Your admin-specific endpoint logic here
        return "Admin endpoint";
    }
}
```

In this example, the @RolesAllowed annotation specifies the roles required to access the corresponding endpoints.

With these steps, you can use users stored in a database for authentication and implement role-based access control in your Quarkus application.

# With Keycloak

To use Keycloak for user authentication and authorization in a Quarkus application, you can follow these steps:

## 1. Set up Keycloak:

Install and configure Keycloak. You can run Keycloak locally or deploy it to a server or cloud platform.
Create a realm, clients, roles, and users in Keycloak according to your application's requirements.

## 2. Configure Quarkus:

In your Quarkus project, add the Keycloak extension as a dependency to your pom.xml or build.gradle:
For Maven:

```
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-keycloak</artifactId>
</dependency>
```

## 3. Configure Application Properties:

Configure your Quarkus application to use Keycloak by specifying the Keycloak realm and server information in your application.properties or application.yaml:

```
quarkus.oidc.auth-server-url=http://localhost:8080/auth/realms/your-realm
quarkus.oidc.client-id=your-client-id
quarkus.oidc.credentials.secret=your-client-secret
```

## 4. Secure Endpoints:

Annotate your JAX-RS resources or methods with @RolesAllowed to specify the roles required to access the endpoints:

```
@Path("/api")
@RolesAllowed("USER")
public class MyResource {

    @GET
    @Path("/user")
    public String userEndpoint() {
        // Your user-specific endpoint logic here
        return "User endpoint";
    }

    @GET
    @Path("/admin")
    @RolesAllowed("ADMIN")
    public String adminEndpoint() {
        // Your admin-specific endpoint logic here
        return "Admin endpoint";
    }
}
```

## 5. Secure Quarkus Applications using Keycloak:

Implement Keycloak authentication in your Quarkus application by following Keycloak's authentication and authorization flow. Your application will redirect users to the Keycloak login page when they try to access secured endpoints.
## 6. Testing:

Test your secured endpoints by running your Quarkus application and accessing them with valid Keycloak user credentials. Ensure that users with the appropriate roles can access the expected endpoints.
## 7. Role Mapping:

In Keycloak, make sure to map roles to your users or clients correctly so that the @RolesAllowed annotation works as expected in your Quarkus application.
By following these steps, you can integrate Keycloak with your Quarkus application to handle user authentication and authorization using OIDC (OpenID Connect) standards. Keycloak simplifies identity and access management and provides a secure and scalable solution for securing your Quarkus APIs and applications.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
