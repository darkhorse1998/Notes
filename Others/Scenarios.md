# Scenarios

## Migrating Monolith to Microservices

Whether monolithic structure is fruitful or it needs to be brokn down into microservice, depends on various factors like complexity, business logic etc. If the business use case or complexity is lesser, and the application is straightforward, then monolith makes sense. \
If there is a certain funcionality or business logic within a monolithic application, which is being used a lot and leading to a lot of traffic on the server, we might want to isolate this particular business logic out of the monolithic structue, so that we can scale it horizontally and stabilize the workload. This will ensure that we don't need to scale the entire monolith to handle the load; instead we just scale the particular business logic deployed as a microservice to handle the traffic outbursts.

### Dividing the Application into Layers

Presentation layer – elements that manage HTTP requests and use either a (REST) ​​API or an HTML-based web interface. In an application with a complex user interface, the presentation layer is frequently a significant amount of code.

Business logic layer – elements that are the nucleus of the application and apply business rules.

Data access layer – elements that access infrastructure parts such as databases and message brokers.

In this strategy, the presentation layer is the most typical candidate that can be transformed into a microservice. Such decoupling of the monolith has two main advantages. It allows you to create, deploy, and scale front- and back-end functionality independently from each other. Besides, it also allows the presentation-layer developers to quickly iterate the user interface and smoothly carry out microservices testing.

> Source: [Monolith to Microservices](https://sigma.software/about/media/migrating-monolith-microservices-step-step-guide)
