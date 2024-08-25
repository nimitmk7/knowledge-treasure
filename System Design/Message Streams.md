Message Streams are a type of [[Message Brokers]].

Whenever there is a use case where we want the same message to be processed by 2 different types of consumer, we use message streams instead of [[Message Queues]].

Each type of consumer independently iterates over the messages in the queue and processes them. The message remains in the queue till its retention period.

**Examples**: Apache Kafka, AWS Kinesis



