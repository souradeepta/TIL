# AWS Simple Queue Service



Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted                                    queue that lets you integrate and decouple distributed software systems and components. Amazon SQS offers common constructs such as [dead-letter queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) and [cost allocation tags](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-queue-tags.html). It provides a generic web services API and it can be accessed by any programming language that the AWS SDK                                    supports.                                 

Amazon SQS supports both [standard](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html) and [FIFO queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html).



## What are the main benefits of Amazon SQS?

- Security – [You control](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-authentication-and-access-control.html) who can  send messages to and receive messages from an Amazon SQS queue.                                          

  [Server-side encryption (SSE)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-server-side-encryption.html) lets  you transmit sensitive data by protecting the contents of messages in queues using keys managed in AWS Key Management Service (AWS KMS).                                          

- Durability – To ensure the safety of your messages, Amazon SQS stores them on multiple servers. Standard queues support [at-least-once message delivery](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html#standard-queues-at-least-once-delivery), and FIFO queues support [exactly-once message processing](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html#FIFO-queues-exactly-once-processing).                                          

- Availability – Amazon SQS uses [redundant infrastructure](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-basic-architecture.html) to provide highly-concurrent access to messages and high availability for producing and  consuming messages.                                           

- Scalability – Amazon SQS can process each [buffered request](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-client-side-buffering-request-batching.html)  independently, scaling transparently to handle any load increases or spikes  without any provisioning instructions.                                          

- Reliability – Amazon SQS locks your messages during  processing, so that multiple producers can send and multiple consumers can  receive messages at the same time.                                           

- Customization – Your queues don't have to be  exactly alike—for example, you can [set a  default delay on a queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-delay-queues.html). You can store the contents of messages  larger than 256 KB [using Amazon Simple Storage Service (Amazon S3)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-s3-messages.html)  or Amazon DynamoDB, with Amazon SQS holding a pointer to the Amazon S3 object, or you can split  a large message into smaller messages.                                          



## Basic Amazon SQS architecture



## Distributed queues

There are three main parts in a distributed messaging system: the components of  your distributed system, your queue (distributed on Amazon SQS servers), and the messages  in the queue.                                 

In the following scenario, your system has several *producers* (components that send messages                                    to the queue) and *consumers* (components that receive messages from the queue). The queue (which                                    holds messages A through E) redundantly stores the messages across multiple Amazon                                    SQS servers.                                 

​                                                                         ![img](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/ArchOverview.png)                                                                                                          



## Message lifecycle

The following scenario describes the lifecycle of an Amazon SQS message in a queue,                                    from creation to deletion.                                 

​                                                                         ![img](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/sqs-message-lifecycle-diagram.png)                                                                                                          

 ![img](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/number-1-red.png) A producer (component 1) sends message A to a queue, and the message is distributed across the Amazon SQS servers redundantly.                                 

 ![img](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/number-2-red.png)  When a consumer (component 2) is ready to process messages, it consumes messages from the queue, and message A is returned. While message A is being processed, it remains in the queue and isn't returned to subsequent receive requests for the duration of the [visibility timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html).                                 

  ![img](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/images/number-3-red.png)   The consumer (component 2) deletes message A from the queue to prevent the message from being received and processed again when the visibility timeout expires.       

 

## Amazon SQS Standard queues                          

Amazon SQS offers *standard* as the default queue type. Standard queues support a nearly unlimited number of API calls per second, per API action   (`SendMessage`, `ReceiveMessage`, or `DeleteMessage`). Standard queues support at-least-once message delivery. However, occasionally (because of the highly distributed architecture that allows nearly unlimited throughput), more than one copy of a message might be delivered   out of order. Standard queues provide best-effort ordering which ensures that messages are generally delivered in the same order as they're sent.  



## Amazon SQS FIFO (First-In-First-Out) queues                     

*FIFO (First-In-First-Out)* queues are designed to enhance messaging between  applications when the order of operations and events is critical, or where duplicates  can't be tolerated, for example:                                 

- Ensure that user-entered commands are executed in the right order.
- Display the correct product price by sending price modifications in the right order.                                          
- Prevent a student from enrolling in a course before registering for an   account.                                          

FIFO queues also provide exactly-once processing but have a limited number of transactions per second (TPS)



## Amazon SQS dead-letter queues                       

Amazon SQS supports *dead-letter queues*, which other queues  (*source queues*) can target for messages that can't be processed  (consumed) successfully. Dead-letter queues are useful for debugging your application  or  messaging system because they let you isolate problematic messages to determine why their processing doesn't succeed.

### How do dead-letter queues work?

Sometimes, messages can’t be processed because of a variety of possible issues,  such as erroneous conditions within the producer or consumer application or an   unexpected state change that causes an issue with your application code. For  example, if a user places a web order with a particular product ID, but the product  ID is deleted, the web store's code fails and displays an error, and the message  with the order request is sent to a dead-letter queue.                                 

When the  `ReceiveCount` for a message exceeds the `maxReceiveCount`                                    for a queue, Amazon SQS moves the message to a dead-letter queue (with its original                                    message ID). For example, if the source queue has a redrive policy with  `maxReceiveCount` set to 5, and the consumer of the source queue   receives a message 6 times without ever deleting it, Amazon SQS moves the message  to the  dead-letter queue.            

### What are the benefits of dead-letter queues?

The main task of a dead-letter queue is handling message failure.   A dead-letter queue lets you set aside and isolate messages that can’t be processed  correctly to determine why their processing didn’t succeed.  Setting up a dead-letter queue allows you to do the following:                                 

- Configure an alarm for any messages delivered to a dead-letter queue.
- Examine logs for exceptions that might have caused messages to be delivered  to a dead-letter queue.                                          
- Analyze the contents of messages delivered to a dead-letter queue to diagnose software                                             or the producer’s or consumer’s hardware issues.                                          
- Determine whether you have given your consumer sufficient time to process messages.

​                     