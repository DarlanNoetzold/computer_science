#  Message Broker with Quarkus

Make sure you have RabbitMQ installed and running on your machine or a reachable server.

## 1. Add Dependencies:

In your pom.xml file, add the necessary dependencies for RabbitMQ and Quarkus:

```
<dependencies>
    <!-- Quarkus Core -->
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-resteasy</artifactId>
    </dependency>

    <!-- Quarkus RabbitMQ -->
    <dependency>
        <groupId>io.quarkus</groupId>
        <artifactId>quarkus-smallrye-reactive-messaging-rabbitmq</artifactId>
    </dependency>
</dependencies>
```

After adding the dependencies, you can use the default RabbitMQ configuration.

## 2. RabbitMQ Configuration:

Create a configuration file (e.g., application.properties or application.yml) and add the following configurations to connect to RabbitMQ:

```
# RabbitMQ Configuration
quarkus.smallrye-reactive-messaging.source.my-source.connector=smallrye-amqp
quarkus.smallrye-reactive-messaging.source.my-source.host=localhost           # RabbitMQ server address
quarkus.smallrye-reactive-messaging.source.my-source.port=5672                # Default port
quarkus.smallrye-reactive-messaging.source.my-source.username=guest           # Default username
quarkus.smallrye-reactive-messaging.source.my-source.password=guest           # Default password
quarkus.smallrye-reactive-messaging.source.my-source.outgoing-payload-as-text=true
```

You can customize these settings as needed for your environment.

## 3. Producer Configuration:

Now, let's create a message producer using Quarkus. For example, let's create a REST service that sends messages to a RabbitMQ topic:

```
import io.smallrye.reactive.messaging.annotations.Channel;
import io.smallrye.reactive.messaging.annotations.Emitter;
import org.eclipse.microprofile.reactive.messaging.Outgoing;

import javax.enterprise.context.ApplicationScoped;

@ApplicationScoped
public class MessageProducer {

    @Inject
    @Channel("my-source")
    Emitter<String> emitter;

    public void sendMessage(String message) {
        emitter.send(message);
        System.out.println("Message sent: " + message);
    }
}
```

## 4. Consumer Configuration:

Now, let's create a message consumer that will receive and process messages from RabbitMQ:

```
import io.smallrye.reactive.messaging.annotations.Channel;
import io.smallrye.reactive.messaging.annotations.Incoming;

import javax.enterprise.context.ApplicationScoped;

@ApplicationScoped
public class MessageConsumer {

    @Incoming("my-source")
    public void receiveMessage(String message) {
        System.out.println("Message received: " + message);
        // Perform necessary processing here
    }
}
```

## 5. Run the Application

Now, you can run your Quarkus application. The producer will send messages to RabbitMQ, and the consumer will receive and process them.

This is a simple example of RabbitMQ configuration with Quarkus. In real-world applications, you may need to configure exchanges, bindings, queues, and more, depending on your system's message requirements. Be sure to read the official documentation of Quarkus and RabbitMQ for detailed configuration and advanced features.



⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
