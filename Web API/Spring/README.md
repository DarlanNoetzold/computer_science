# Example with Spring

Creating a RESTful API with Spring is a common task for many developers. Here's a step-by-step guide on how to create a simple API using Spring Boot, one of the most popular frameworks for building Java applications.

## Step 1: Set Up Your Development Environment
Before you start, make sure you have the following installed:

* Java Development Kit (JDK)
* Integrated Development Environment (IDE) like IntelliJ IDEA or Eclipse
* Maven or Gradle for dependency management
* Postman or a similar tool for testing APIs


## Step 2: Create a New Spring Boot Project
Open your IDE and create a new Spring Boot project.
Choose a group and artifact ID for your project.
Select the Spring Web dependency.
Spring Boot's project setup wizard in most IDEs simplifies this process.

## Step 3: Define a Data Model
In this example, let's create a simple API for managing a list of tasks. Define a data model class for tasks:

```
@Entity
public class Task {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private boolean completed;

    // Getters and setters
}
```

Step 4: Create a Repository
Create a repository interface to interact with the database. You can use Spring Data JPA for this:

```
public interface TaskRepository extends JpaRepository<Task, Long> {
    // Custom queries can go here if needed
}
```

## Step 5: Create a Service
Create a service class to handle business logic:

```
@Service
public class TaskService {
    @Autowired
    private TaskRepository taskRepository;

    public List<Task> getAllTasks() {
        return taskRepository.findAll();
    }

    public Task createTask(Task task) {
        return taskRepository.save(task);
    }

    public Task updateTask(Long id, Task task) {
        if (!taskRepository.existsById(id)) {
            throw new ResourceNotFoundException("Task not found with id: " + id);
        }
        task.setId(id);
        return taskRepository.save(task);
    }

    public void deleteTask(Long id) {
        taskRepository.deleteById(id);
    }
}
```

## Step 6: Create a Controller
Create a REST controller to define API endpoints:

```
@RestController
@RequestMapping("/api/tasks")
public class TaskController {
    @Autowired
    private TaskService taskService;

    @GetMapping
    public List<Task> getAllTasks() {
        return taskService.getAllTasks();
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public Task createTask(@RequestBody Task task) {
        return taskService.createTask(task);
    }

    @PutMapping("/{id}")
    public Task updateTask(@PathVariable Long id, @RequestBody Task task) {
        return taskService.updateTask(id, task);
    }

    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteTask(@PathVariable Long id) {
        taskService.deleteTask(id);
    }
}
```

## Step 7: Run Your Application
You can run your Spring Boot application using the IDE or with the following command:

```
./mvnw spring-boot:run
```
## Step 8: Test Your API
Use Postman or a similar tool to test your API by sending HTTP requests to the defined endpoints:

* GET /api/tasks to retrieve all tasks.
* POST /api/tasks to create a new task.
* PUT /api/tasks/{id} to update an existing task.
* DELETE /api/tasks/{id} to delete a task.


## Step 9: Documentation (Optional)
You can use tools like Swagger or Springfox to generate API documentation for your endpoints.

Congratulations! You've created a simple RESTful API using Spring Boot. You can expand on this foundation by adding authentication, validation, and more complex business logic as needed for your project.









⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
