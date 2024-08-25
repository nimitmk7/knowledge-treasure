They are a type of [[Message Brokers]]. Examples include AWS SQS, RabbitMQ. 

In message queues, all the consumers are equally capable of executing any message. Thus, the consumers are **homogenous**.

Each consumer processes the messages and then continuously pings the queue to check for any new messages. 

