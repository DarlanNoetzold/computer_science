# Example with Quarkus

Creating a RESTful API with Quarkus is a streamlined process due to Quarkus' developer-friendly features and fast boot times. Here's a step-by-step guide on how to create a simple API using Quarkus:

## Step 1: Set Up Your Development Environment
Before you begin, ensure you have the following installed:

Java Development Kit (JDK) 11 or higher
Integrated Development Environment (IDE) like IntelliJ IDEA or Visual Studio Code (optional but recommended)
Apache Maven for dependency management (Quarkus also supports Gradle)
Postman or a similar tool for testing APIs
Step 2: Create a New Quarkus Project
Open your terminal or command prompt.
Navigate to the directory where you want to create your project.
Run the following command to create a new Quarkus project:

```
mvn io.quarkus:quarkus-maven-plugin:2.3.1.Final:create
```

Replace 2.3.1.Final with the latest version if needed.

Follow the interactive prompts to configure your project. Choose the appropriate extensions based on your project requirements. For a basic REST API, you can select the "Resteasy" extension.

## Step 3: Define a Resource
In Quarkus, resources are the classes that handle HTTP requests. Create a simple resource class to define your API:

```
import javax.ws.rs.*;
import javax.ws.rs.core.MediaType;
import java.util.List;

@Path("/tasks")
@Produces(MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
public class TaskResource {

    @GET
    public List<Task> getAllTasks() {
        // Implement logic to retrieve and return tasks
    }

    @POST
    public Response createTask(Task task) {
        // Implement logic to create a task and return a response
    }

    @PUT
    @Path("/{id}")
    public Response updateTask(@PathParam("id") Long id, Task task) {
        // Implement logic to update a task and return a response
    }

    @DELETE
    @Path("/{id}")
    public Response deleteTask(@PathParam("id") Long id) {
        // Implement logic to delete a task and return a response
    }
}
```

## Step 4: Define Your Model
Define a data model for tasks:

```
public class Task {
    private Long id;
    private String title;
    private boolean completed;

    // Getters and setters
}
```
## Step 5: Implement the Business Logic
Implement the business logic for your API by adding methods to handle tasks.

## Step 6: Run Your Quarkus Application
You can run your Quarkus application with the following command:

```
./mvnw quarkus:dev
```

This command starts your application in development mode, which enables hot-reloading for code changes.

## Step 7: Test Your API
Use Postman or a similar tool to test your API by sending HTTP requests to the defined endpoints (e.g., GET /tasks, POST /tasks, PUT /tasks/{id}, DELETE /tasks/{id}).

## Step 8: Documentation (Optional)
You can add documentation to your API using tools like Swagger, Quarkus Rest Client, or Quarkus OpenAPI.

Congratulations! You've created a simple RESTful API using Quarkus. Quarkus offers various extensions and features for building robust and efficient applications, making it a versatile choice for developing APIs and microservices. You can expand on this foundation by adding authentication, validation, and more complex business logic as needed for your project.


⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
