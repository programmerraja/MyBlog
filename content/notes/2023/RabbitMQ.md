---
title: RabbitMQ Notes
date: 2023-12-08T18:26:15+05:30
draft: false
tags:
  - rabbitmq
---
## What  is RabbitMQ?

RabbitMQ is a popular open-source message broker that implements the AMQP (Advanced Message Queuing Protocol) standard.

## Channel

A channel is a separate communication channel within a single connection. Channels allow multiple concurrent exchanges to be executed in parallel, providing a way to separate different parts of your application.

Each channel has its own set of resources, such as queues, exchanges, and bindings, which are independent of the resources used by other channels in the same connection. This allows you to manage concurrency and improve performance by isolating different parts of your application into separate channels.

For example, you might create one channel for sending messages and another channel for receiving messages, or create separate channels for different types of messages or different parts of your application. By using multiple channels, you can take advantage of the scalability and performance benefits of AMQP while also making it easier to manage your application.

## Channel vs connection

In RabbitMQ, a connection represents a network connection to the RabbitMQ 
broker. When a client connects to RabbitMQ, it establishes a TCP connection to the broker. This connection remains open until the client explicitly closes it or the broker closes it due to a network error or a timeout. A connection is created using the AMQP protocol, and it is responsible for authentication, connection handling, and connection-level flow control.

On the other hand, a channel is a virtual connection inside a connection that allows multiple logical connections to be multiplexed over a single physical connection. When a client establishes a connection to RabbitMQ, it can create one or more channels inside that connection. Each channel is a separate AMQP session that can be used to publish or consume messages, declare queues and exchanges, and bind queues to exchanges.

The main difference between a connection and a channel is that a connection represents a physical connection to the broker, whereas a channel represents a logical connection within that physical connection. Channels allow multiple AMQP operations to be multiplexed over a single network connection, which can help reduce the overhead of establishing multiple network connections.

In summary, a connection in RabbitMQ represents a physical network connection to the broker, while a channel represents a logical connection within that physical connection, allowing multiple AMQP operations to be multiplexed over a single network connection.

## Queue 

### Durable Queue
A queue that survives broker restarts.Queue metadata (not the messages themselves, unless explicitly persisted) is stored on disk. Messages in the queue can be persisted if they are marked as persistent.

### Transient Queues

A **transient queue** refers to a queue that is not durable, meaning it is not persistent and will not survive a broker restart. Transient queues are typically used for short-lived or temporary messaging scenarios, where persistence and reliability are not critical. which has performance higher compared to normal queue 

### Exclusive Queue
A queue that is private to the connection that declared it and is deleted when the connection closes.

### Lazy Queue
A queue designed to handle large volumes of messages by storing them on disk instead of memory.
- Created with a `x-queue-mode` argument set to `lazy`.
- Messages are moved to disk as soon as possible.
- Helps prevent memory overload in scenarios with high message backlogs.

###  Quorum Queue
 A replicated queue type that uses the Raft consensus algorithm for high availability and consistency.
 - Messages and metadata are replicated across multiple nodes in a cluster.
 - Provides stronger data safety guarantees than classic mirrored queues.
 - Supports sharding for large-scale message handling.

|**Queue Type**|**Persistent**|**Replicated**|**Special Feature**|
|---|---|---|---|
|Durable Queue|Yes|No|Survives restarts|
|Transient Queue|No|No|Lost after restart|
|Exclusive Queue|Optional|No|Tied to a single connection|
|Auto-Delete Queue|Optional|No|Deleted when last consumer disconnects|
|Temporary Queue|Optional|No|Auto-generated, often exclusive|
|Lazy Queue|Yes|No|Stores messages on disk to save memory|
|Quorum Queue|Yes|Yes|High availability using Raft|
|Classic Mirrored Queue|Yes|Yes|Legacy replication|
|Dead Letter Queue|Optional|Optional|For undeliverable messages|
|Header Queue|Optional|Optional|Matches on message headers|

## Exchanges 

In RabbitMQ, an exchange is a message routing agent that receives messages from producers and routes them to queues based on message properties such as the routing key. When a producer sends a message to an exchange, it is up to the exchange to route the message to one or more queues.

There are four types of exchanges in RabbitMQ:

1. `Direct exchange`: Messages are routed to queues based on the exact match between the routing key of the message and the routing key of the queue.
    
2. `Fanout exchange`: Messages are routed to all the queues bound to the exchange. It ignores the routing key and sends messages to all the queues that are bound to the exchange.
    
3. `Topic exchange`: Messages are routed to queues based on pattern matching between the routing key of the message and the routing key of the queue. It uses wildcards to match the routing key.
    
4. `Headers exchange`: Messages are routed to queues based on header values instead of routing keys. It is rarely used, and its functionality is similar to the topic exchange.
    
5. `x-delayed-message` → is a custom exchange type in RabbitMQ that allows you to delay messages before they are delivered to a queue. This exchange type is not included in RabbitMQ by default and must be installed as a plugin.
    
    When you create an **`x-delayed-message`** exchange, you can set a delay time for messages using the **`x-delay`** header. The exchange will hold the message for the specified delay time and then deliver it to the appropriate queue. This is useful in scenarios where you want to delay the delivery of a message until a certain time or after a certain event has occurred.

Example 

```js
const amqp = require('amqplib');

async function setup() {
  // Connect to RabbitMQ
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  // Install the delayed message exchange plugin (if necessary)
  await channel.assertExchange('amq.delayed', 'x-delayed-message', {
    durable: true,
    arguments: {
      'x-delayed-type': 'direct'
    }
  });

  // Create the queue and bind it to the delayed message exchange
  const queueName = 'my-queue';
  const routingKey = 'my-routing-key';

  await channel.assertQueue(queueName, { durable: true });

  await channel.bindQueue(queueName, 'amq.delayed', routingKey);

  // Consumer function to handle incoming messages
  const consumerFunction = (msg) => {
    console.log(`Received message: ${msg.content.toString()}`);
    channel.ack(msg);
  };

  // Consume messages from the queue
  channel.consume(queueName, consumerFunction);

  // Producer function to send messages with a delay
  const producerFunction = async (message, delayMs) => {
    const headers = { 'x-delay': delayMs };
    const buffer = Buffer.from(message);

    await channel.publish('amq.delayed', routingKey, buffer, { headers });

    console.log(`Sent message "${message}" with delay of ${delayMs}ms`);
  };

  // Send some messages with a delay
  await producerFunction('Hello World!', 5000);
  await producerFunction('Delayed message!', 10000);
}

setup();
```

**Application of Exchange**

Let's say you have a distributed system that consists of multiple microservices. Each microservice has its own queue, and they communicate with each other through messaging.

When a microservice wants to send a message to another microservice, it can publish the message to an exchange instead of directly sending it to the other microservice's queue. The exchange is responsible for routing the message to the appropriate queue based on the message's routing key.

For example, let's say you have a microservice that handles user authentication, and another microservice that handles user orders. When a user logs in, the authentication microservice can publish a message to the exchange with a routing key of "user.login". The exchange can then route this message to the order microservice's queue, which is bound to the exchange with a matching routing key.

If you didn't use an exchange, the authentication microservice would have to know the exact name of the order microservice's queue and send the message directly to that queue. This would tightly couple the microservices and make it harder to make changes to the system in the future.

By using an exchange, you can create a loosely coupled system where microservices don't need to know about each other's queues. This makes it easier to add new microservices or change the routing rules of messages without affecting other parts of the system.


## Durablity

RabbitMQ provides durability by persisting messages and metadata to disk. This 
ensures that messages are not lost in case of a server failure.if it durable to true it will presist if we set false it will not until it reach threshold

## Prefetch

RabbitMQ uses prefetch to control the amount of messages a consumer can consume at once. Prefetch specifies the number of unacknowledged messages that can be in-flight before the broker stops delivering more messages to the consumer. This avoids overloading a consumer with too many messages at once.


## Amqp-connection-manager vs Amqplib

amqp-connection-manager and amqplib are both Node.js libraries for working with RabbitMQ, but they have different purposes and use cases.

amqplib is a low-level RabbitMQ client library that provides a thin wrapper around the RabbitMQ API. It allows you to send and receive messages, create and manage exchanges and queues, and interact with other RabbitMQ features. amqplib provides a direct and flexible interface to RabbitMQ, and is a good choice if you need complete control over your RabbitMQ interactions.

amqp-connection-manager, on the other hand, is a higher-level library that provides connection management and channel pooling. It uses amqplib under the hood, but adds features like connection retry, connection throttling, and automatic channel recovery. amqp-connection-manager is a good choice if you want to simplify your RabbitMQ code and reduce the chance of connection errors, or if you need to handle multiple connections and channels.

In general, if you need fine-grained control over your RabbitMQ interactions, or if you have a small number of connections and channels, amqplib is a good choice. If you have a large number of connections and channels, or if you want to simplify your RabbitMQ code and reduce the chance of connection errors, amqp-connection-manager is a better choice.


## What are the best practice regarding channel and connection

1. Use a single connection per application instance: It's a good practice to use a single connection for an entire application instance. Creating multiple connections can lead to resource wastage, and can make it difficult to manage and monitor connections.
2. Use a connection pool: Creating and closing connections can be expensive, so it's recommended to use a connection pool. Connection pools can be used to manage the number of connections and can help improve performance.
3. Use a separate channel for each thread: When creating a multithreaded application, use a separate channel for each thread instead of sharing a single channel. Sharing a single channel between threads can lead to contention issues and can cause the application to become unstable.
4. Close channels when they're no longer needed: It's a good practice to close channels when they're no longer needed. This helps to reduce the number of open channels and frees up resources.
5. Use a lightweight protocol: RabbitMQ provides AMQP, which is a lightweight protocol that is designed for message queuing. Using a lightweight protocol can help improve performance and reduce the overhead of managing connections and channels.
6. Use transactional channels: When sending multiple messages, it's a good practice to use transactional channels. This ensures that all messages are either sent successfully or not sent at all.
7. Use a connection heartbeat: RabbitMQ provides a connection heartbeat mechanism that can be used to detect network failures. It's a good practice to use connection heartbeat to ensure that the application can recover from network failures.
8. Use connection and channel events: RabbitMQ provides connection and channel events that can be used to monitor and manage connections and channels. Using these events can help improve the reliability and performance of the application.


## How to Handle failures

If we want to retry if the consumer get failed when processing the data we can have 2 way
    1. nack
    2. reject

**Difference between them**

The main difference between nack (negative acknowledgement) and reject in RabbitMQ is how they handle message rejection and requeuing:

1. `nack` (channel.nack):
    - `nack` is used to negatively acknowledge a message and reject it.
    - It allows you to control whether the message should be requeued or discarded.
    - The method signature is `channel.nack(message, allUpTo, requeue)`.
    - The `message` parameter represents the message being rejected.
    - The `allUpTo` parameter is a boolean that indicates whether all unacknowledged messages prior to the given message should also be rejected.
    - The `requeue` parameter is a boolean that determines whether the rejected message should be requeued or discarded.
    - With `channel.nack`, you have more control over requeuing behavior and can choose whether to discard the message or requeue it for retry.
		
2. `reject` (channel.reject):
    - `reject` is used to reject a message without acknowledging it.
    - When a message is rejected using `reject`, it can optionally be requeued based on the `requeue` parameter.
    - The method signature is `channel.reject(message, requeue)`.
    - The `message` parameter represents the message being rejected.
    - The `requeue` parameter is a boolean that determines whether the rejected message should be requeued or discarded.
    - By default, if `requeue` is set to `true`, the message will be requeued for future delivery. If set to `false`, the message will be discarded. 

In summary, nack provides more flexibility by allowing you to explicitly control requeuing behavior (requeue or discard), whereas reject gives you the option to requeue the message based on the requeue parameter. Both methods can be used to handle message rejection and retry scenarios in RabbitMQ, depending on your specific requirements.

Rabbitmq simulator tool to playaround with it

## Unack msg

In RabbitMQ, messages are marked as **unacknowledged (unack)** when a consumer receives a message but hasn't sent an acknowledgment back to RabbitMQ yet. Unacknowledged messages remain in the queue and are not re-delivered to other consumers until they are either acknowledged or the connection with the consumer is closed.


## Plugin

Shovel Plugin  RabbitMQ plugin that unidirectionally moves messages from a source to a destination. rabbitmq
## Internal

Protocol : AMQP

Blog 
1. how does elrang does  scheduling





## Resources
1. [https://www.cloudamqp.com/blog/part1-rabbitmq-best-practice.html](https://www.cloudamqp.com/blog/part1-rabbitmq-best-practice.html)
2. [https://www.cloudamqp.com/blog/part2-rabbitmq-best-practice-for-high-performance.html](https://www.cloudamqp.com/blog/part2-rabbitmq-best-practice-for-high-performance.html)
3. [ What we learned form running 1k queue Cloudamp]()
	1. Keep connection and channle as low as possible
	2. seprate connection for pub and consume4
	3. dont have too large queue 10k msg
	4. Enable lazy queue (loaded on ram when needed)
	5. rabbitmq sharding
	6. limited use on priroity queue
	7. adjust prefetch (cloudampq default 1k )
5. [Explain basic and how to handle error case]([https://medium.com/@upadhyayyuvi/building-a-reliable-message-queue-system-with-rabbitmq-and-node-js-step-by-step-guide-275e026d2090](https://medium.com/@upadhyayyuvi/building-a-reliable-message-queue-system-with-rabbitmq-and-node-js-step-by-step-guide-275e026d2090))
6. [https://www.cloudamqp.com/blog/when-to-use-rabbitmq-or-apache-kafka.html](https://www.cloudamqp.com/blog/when-to-use-rabbitmq-or-apache-kafka.html)
7. https://youtu.be/HzPOQsMWrGQ 


https://github.com/sensu-plugins/sensu-plugins-rabbitmq