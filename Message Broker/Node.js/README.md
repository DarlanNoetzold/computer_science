# Message Broker with Node.js


## 1. Install Dependencies

You will need the amqplib library to interact with RabbitMQ. You can install it using npm:
```
npm install amqplib
```

## 2. RabbitMQ Configuration

Create a configuration file to store the RabbitMQ connection information (e.g., rabbitmq-config.js):

```
module.exports = {
  rabbitmq_url: 'amqp://localhost', // RabbitMQ connection URL
};
```

You can customize this URL as needed for your environment.

## 3. Message Producer (Publisher)

Here is an example of how to create a message producer (publisher) that sends messages to a queue:

```
const amqp = require('amqplib');
const rabbitmqConfig = require('./rabbitmq-config');

async function sendMessage(message) {
  try {
    const connection = await amqp.connect(rabbitmqConfig.rabbitmq_url);
    const channel = await connection.createChannel();
    const queueName = 'my-queue'; // Queue name

    await channel.assertQueue(queueName, { durable: false });
    channel.sendToQueue(queueName, Buffer.from(message));

    console.log(`Message sent: ${message}`);
    
    setTimeout(() => {
      connection.close();
    }, 500);
  } catch (error) {
    console.error('Error sending message:', error);
  }
}

// Example usage:
sendMessage('This is an example message.');
```

## 4. Message Consumer

Here is an example of how to create a message consumer that receives and processes messages from the queue:

```
const amqp = require('amqplib');
const rabbitmqConfig = require('./rabbitmq-config');

async function receiveMessage() {
  try {
    const connection = await amqp.connect(rabbitmqConfig.rabbitmq_url);
    const channel = await connection.createChannel();
    const queueName = 'my-queue'; // Queue name

    await channel.assertQueue(queueName, { durable: false });
    console.log('Waiting for messages...');

    channel.consume(queueName, (message) => {
      if (message !== null) {
        const content = message.content.toString();
        console.log(`Message received: ${content}`);
        // Perform necessary processing here
        channel.ack(message);
      }
    });
  } catch (error) {
    console.error('Error receiving messages:', error);
  }
}

// Example usage:
receiveMessage();
```
## 5. Run the Application

Now you can run your Node.js application. The producer will send messages to the queue, and the consumer will receive and process them.

This is a simple example of RabbitMQ configuration with Node.js. In real-world applications, you may need to configure exchanges, bindings, topics, and more, depending on your system's message requirements. Be sure to read the official documentation of RabbitMQ and the amqplib library for detailed configuration and advanced features.


⭐️ From [DarlanNoetzold](https://github.com/DarlanNoetzold)
