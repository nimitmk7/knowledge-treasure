![[Pasted image 20240503083806.png]]
## Premise
Modern applications are getting more and more complex. 
1. Time and resource-consuming operations
2. Communication between multiple services
3. Processing lots of data 

– that’s only a few of many problems that developers have to face.

## What is a message broker ?
A **message broker** is a piece of software, which enables services and applications to communicate with each other using messages. The message structure is formally defined and independent from the services that send them.

> [!quote] **This allows applications to share information with one another, even if they’re written in different programming languages!**

## Basic Concepts

### Producer
It is the application responsible for **sending messages**. It’s connected with the message broker. In publish/subscribe pattern, they are called `publishers`.

### Consumer
The endpoint that consumes messages waiting in the message broker. In publish/subscribe pattern they are called `subscribers`.

### Queue/Topic
A folder in a filesystem. Message broker uses them to store messages.

## Distribution Patterns
In this pattern, there is **one-to-one relation set between the sender and the receiver of the message**. Each message is sent and consumed only once. We can use this pattern for example, when some action needs to be performed only once.

>[!faq] What’s the difference between this and some REST API ? 
> The answer is simple. The message broker guarantees that message will not be lost in case of failure of the consumer. It’s stored safely in the message broker queue.

![Message broker distribution: point-to-point messaging](message-broker-example-1.png)

### Publish/Subscribe
Here, **the sender of the message doesn’t know anything about receivers**. The message is being sent to the topic. After that, it’s distributed among all endpoints subscribed to that topic. 

It can be useful e.g. for implementing notifications mechanism or distributing independent tasks. This solution can also lead to implementing an **event-driven architecture-based system, where applications have fewer dependencies between each other**.

In this pattern, the components are loosely coupled and transmit events to one another. These events would be messages sent to the message broker.

![Message broker distribution: Pub/Sub](message-broker-example-2.png)

## Advantages
- **Provided communication between services that may not be running at the same time**.  
	- The producer can send messages regardless of whether the consumer is active or not. All it needs is a running message broker. The same applies to the consumer.
- **Improved system performance by introducing asynchronous processing.**  
	- High-consuming tasks can be distributed to separate processes. That will fasten up your application and increase user experience.
- **Increased reliability by guaranteeing the transmission of messages.**
	- Message brokers offer a redelivering mechanism. In case of consumer failure, it can redeliver the message immediately or after some specified time. It also supports routing of not delivered messages – it’s called a **dead-letter** mechanism.

## Disadvantages
Use of the message broker involves asynchronous processing. Therefore, the disadvantages of using it are related to the challenges we face by using asynchronous calls.

- **Increased system complexity**. 
	- Introducing a message broker in your system is a new element to your whole system architecture. Because of that, there are more things you have to take into account, such as **maintaining the network between components** or **security issues**.  Additionally, a new problem arises related to eventual consistency. Some components could not have up-to-date data until the messages are propagated and processed.
- **Debugging can be harder.**  
	- Let’s say you have async multiple stages of processing a single request using the message broker. You send something but did not receive a notification. Searching for a cause of failure can be a challenge as every service has its own logs. Keep in mind to add some message tracing facilities alongside implementing systems using message broker.
- **There’s a steep learning curve at first.**  
	- Message brokers are not trivial as they sound. They have a lot of setup and configuration options. The size of queues and messages, the behavior of queues, delivery settings or messages TTL, are only a few of many options you can set.

## When to use ?

### 1. Long-running tasks and crucial API 

If you have actions in your system which:

- are time and resource-consuming 
- doesn’t require you to return the result of the operation immediately

…then the message broker is for you.

![Message Brokers vs API](message-broker-example-3.png)

### 2. Microservices

Large systems can consist of many separate services. It can be c**hallenging to coordinate communication** between them. The solution which comes at first is to make integration using REST API. 

However, with the growing number of services in the system, it can be hard to extend and maintain it. Besides, what if one of your applications goes down and becomes unavailable? The API will start returning critical errors. 

A great alternative is to create **event-based communication** and use the message broker together with publish/subscribe pattern. The broker would work as a **central router** (to route messages). Every service can subscribe to the types of messages that they need.

>[!quote] **This solution has a lot of benefits. You don’t need to extend your already existing services if you want to add a new one. If some services are down – they will consume the messages after they’ll reboot.**
> 

![Message Brokers vs Microservices](message-broker-example-4.png)

### 3. Mobile Applications
It is a great use case for the message broker. Why? Because almost every mobile app has push notifications, of course. Let’s imagine you are developing a news application. Every phone connected to the network can subscribe to some message broker’s topic. Whenever some redactor posts some news – you are receiving notification.

### 4. Transactional Systems
Sometimes it is required to perform some actions in a specific order. Every action should be triggered after the previous one is finished. 

Every service can have the role of sender and receiver at the same time. After consuming its task it can send a message to the broker that the job’s finished. After that, the next service can take over the video.

![Message Brokers vs Transactional Systems](message-broker-example-5.png)

### 5. Controlling Data Feeds
It can be useful if you want to control and limit the number of some entities in your system. One of the simplest scenarios is to control the limit of registered users. 

You can, of course, do this without the message broker – for example by locking tables on the database layer to prevent race hazard. With message broker it’s more simple – you can just publish a message after every registration request. 

The consumer will take the messages one by one. When it reaches the users limit – it can return some error or send an e-mail to not registered user.

## Examples
- RabbitMQ
- Kafka
- Redis
- Amazon SQS
- Amazon SNS

## References
1. https://tsh.io/blog/message-broker/
2. 
