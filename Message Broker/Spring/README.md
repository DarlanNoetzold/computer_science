# Message Broker with Spring

### Prerequisites:

Make sure you have RabbitMQ installed and running on your machine or a reachable server.
## Add Dependencies:

In your pom.xml file, add the necessary dependencies for RabbitMQ and Spring AMQP:

```
<dependencies>
    <!-- Spring Boot Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <!-- Spring AMQP (RabbitMQ) Starter -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-amqp</artifactId>
    </dependency>
</dependencies>
```

After adding the dependencies, you can use the default RabbitMQ configuration.

## RabbitMQ Configuration:

Create a configuration file (e.g., application.properties or application.yml) and add the following configurations to connect to RabbitMQ:

```
# RabbitMQ Configuration
spring.rabbitmq.host=localhost           # RabbitMQ server address
spring.rabbitmq.port=5672                # Default port
spring.rabbitmq.username=guest           # Default username
spring.rabbitmq.password=guest           # Default password
```

You can customize these settings as needed for your environment.

## Producer Configuration:

Now, let's create a message producer using Spring. For example, let's create a service that sends messages to a queue:

```
import org.springframework.amqp.core.Queue;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MessageProducer {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Autowired
    private Queue queue;  // Queue name

    public void sendMessage(String message) {
        rabbitTemplate.convertAndSend(queue.getName(), message);
        System.out.println("Message sent: " + message);
    }
}
```
## Consumer Configuration:

Now, let's create a message consumer that will receive and process messages from the queue:

```
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

@Service
public class MessageConsumer {

    @RabbitListener(queues = "${queue.name}")
    public void receiveMessage(String message) {
        System.out.println("Message received: " + message);
        // Perform necessary processing here
    }
}
```

## Run the Application:

Now, you can run your Spring application. The producer will send messages to the queue, and the consumer will receive and process them.

Please note that this is a simple example of RabbitMQ configuration with Spring. In real-world applications, you may need to configure exchanges, bindings, topics, and more, depending on your system's message requirements. Be sure to read the official documentation of RabbitMQ and Spring AMQP for detailed configuration and advanced features.

⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
