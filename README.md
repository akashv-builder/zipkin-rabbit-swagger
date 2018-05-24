# zipkin-rabbit-swagger

Contains Eureka Server, Zuul,Config Server and a servie which interact with rabbitmq.

Messaging Concepts The eventing solution presented in this article makes use of some messaging concepts that are described in the following sections.

Producer A producer is simply a piece of software that sends a message to a message broker, for example a customer service in a system of microservices that wants to tell other services that a new customer was created by sending the event customer.created that contains the newly created customers’ ID as a payload.

Consumer A consumer is a piece of software that receives messages from a message broker and processes those messages. In our example, this might be an order service that needs the address of all customers to create orders for those customers. It would process the customer.created event by reading the ID from the event and calling the customer service to load the corresponding customers’ address.

Queue A queue is first-in-first-out message store. The messages are put into a queue by a producer and read from it by a consumer. Once a message is read, it is consumed and removed from the queue. A message can thus only be processed exactly once.

Exchange An exchange is a concept that is part of the AMQP protocol. Basically, it acts as an intermediary between the producer and a queue. Instead of sending messages directly to a queue, a producer can send them to an exchange instead. The exchange then sends those messages to one or more queues following a specified set of rules. Thus, the producer does not need to know the queues that eventually receive those messages.

Binding A binding connects a queue to an exchange. The exchange forwards all messages it receives to the queues it is bound to. A binding can contain a routing key that specifies which events should be forwarded. For example, a binding might contain the routing key customer.* meaning that all events whose type starts with customer. will be routed to the specified queue.

An Event Messaging Concept with AMQP Using the concepts above, we can create an eventing solution with RabbitMQ. The solution is depicted in the figure below.

Eventing with RabbitMQ Event producer and consumers are loosely coupled since an exchange serves as intermediary. Each service cluster gets its own queue. This is necessary since not all events are relevant to each service cluster. An order service may be interested in all customer events (customer.) whereas an archiving service may be interested in all events where an object has been deleted (.deleted). If we had only one queue for all events that queue would sooner or later overflow since it might contain events that no consumer is interested in.

Each consuming service cluster binds its queue the central exchange with a routing key that specifies which events it is interested in. Only those events are then routed into the queue. The events are then consumed by exactly one of the service instances connected to that queue.

The event producing services only need to know the central exchange and send all events to that exchange. Since the consuming services take care of the binding and routing, we have a real, loosely coupled eventing mechanism.


swagger ui is up and running for all microservices

Overall Url- localhost:8765/swagger-ui.html

Individual- localhost:8765/service-name/swagger-ui.html
