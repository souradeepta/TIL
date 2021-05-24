# Microservices Cheat Sheet

1. Do only one thing and do it well. The "one thing" is defined by a "Bounded Context" in Domain-Driven Design (DDD).
2. Own your own data. No shared data stores.
3. Embrace eventual consistency.
   - Don't read your writes.
   - Publish your own state-changes (minimally) to an event log.
4. Leverage event logging and/or streaming to replicate and denormalize data from other services.
   - The event log must be subscribable to *publish* events to subscribers.
   - The event log must be durable and must not lose messages for publishing and replay.
   - Examples: [Kafka](http://kafka.apache.org/), [Confluent.io](http://www.confluent.io/), [Amazon Kinesis](https://aws.amazon.com/kinesis/).
5. When aggregating microservice calls, use asynchronous, non-blocking I/O. Perform as much as possible asynchronously.
6. Support a registry/discovery mechanism.
   - Examples: [Consul](https://www.consul.io/), [Eureka](https://github.com/Netflix/eureka)
7. Support Location Transparency.
   - Support statelessness--no sticky sessions.
8. Support the use of back-pressure (see, Reactive Streams specification) to avoid cascading failures.
   - Examples: [RxJava](https://github.com/ReactiveX/RxJava/wiki/Backpressure)
9. Support the Circuit Breaker pattern to manage faulty service dependencies.
   - Examples: [Hystrix](https://github.com/Netflix/Hystrix), [Apache Zest](https://zest.apache.org/java/2.1/library-circuitbreaker.html)
10. Consider CQRS to scale reads separately from writes.