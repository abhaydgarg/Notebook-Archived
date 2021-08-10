# Decomposing complex applications

## Monolithic architecture

The term monolithic might make us think of a system without modularity, where all the services of an application are interconnected together and almost indistinguishable. However, this is not always the case. Often, monolithic systems have a highly modular architecture and a good decoupling between their internal components.

A perfect example is the Linux Operating System kernel, which is part of a category called monolithic kernels (in perfect opposition with its ecosystem and the Unix philosophy). Linux has thousands of services and modules that we can load and unload dynamically even while the system is running. However, they all run in kernel mode, which means that a failure in any of them might bring the entire OS down (have you ever seen a kernel panic?). This approach is opposed to the microkernel architecture, where only the core services of the operating system run in kernel mode, while the rest is running in user mode, usually each one with its own process. The main advantage of this approach is that a problem in any of these services would more likely cause it to crash in isolation instead of affecting the stability of the entire system.

![Monolithic](./assets/monolithic.png)

The preceding figure shows the architecture of a typical e-commerce application. Its structure is modular; we have two different frontends, one for the main store and another for the administration interface. Internally, we have a clear separation of the services implemented by the application, each one responsible for a specific portion of its business logic: Products, Cart, Checkout, Search, and Authentication and Users. However, the preceding architecture is monolithic, every module, in fact, is part of the same codebase and runs as part of a single application. A failure in any of its components, for example, an uncaught exception, can potentially tear down the entire online store.

Another problem with this type of architecture is the interconnection between its modules; the fact that they all live inside the same application makes it very easy for a developer to build interactions and coupling between modules. For example, consider the use case when a product is being purchased; the Checkout module has to update the availability of the Product object, and if those two modules are in the same application, it's too easy for a developer to just obtain a reference to a Product object and update its availability directly. Maintaining a low coupling between internal modules is very hard in monolithic application, partly because the boundaries between them are not always clear or properly enforced.

## The Microservice architecture

The idea is to break down an application into its essential components, creating separate, independent applications. It is practically the opposite of the monolithic architecture.

The Microservice architecture is probably today the reference pattern for this type of approach, where a set of self-sufficient services replaces big monolithic applications. The prefix micro means that the services should be as small as possible, but always within reasonable limits. Don't be misled by thinking that creating an architecture with a hundred different applications exposing only one web service is necessarily a good choice. In reality, there is no strict rule on how small or big a service should be, it's not the size that matters in the design of a Microservice architecture; instead, it's a combination of different factors, mainly loose coupling, high cohesion, and integration complexity.

![Microservice](./assets/microservice.png)

As we can see from the previous figure, each fundamental component of the e-commerce application is now a self-sustaining and independent entity, living in its own context, with its own database. In practice, they are all independent applications exposing a set of related services (high cohesion).

The data ownership of a service is an important characteristic of the Microservice architecture. This is why the database also has to be split to maintain the proper level of isolation and independence. If a unique shared database is used, it would become much easier for the services to work together; however, this would also introduce a coupling between the services (based on data), nullifying some of the advantages of having different applications.

The dashed line connecting all the nodes tells us that, in some way, they have to communicate and exchange information for the entire system to be fully functional. As the services do not share the same database, there is more communication involved to maintain the consistency of the whole system. For example, the Checkout application needs to know some information about Products, such as the price and restrictions on shipping, and at the same time, it needs to update the data stored in the Products service, for example, the product availability when the checkout is complete.

The main technical advantage of having each service living in its own application context is that crashes, bugs, and breaking changes do not propagate to the entire system. The goal is to build truly independent services that are smaller, easier to change, or even rebuild from scratch.

At this point, it would look like microservices are the solution to all our problems; however, this is far from being true. In fact, having more nodes to manage introduces a higher complexity in terms of integration, deployment, and code sharing; it fixes some of the pains of traditional architectures but it also opens many new questions. How do we make the services interact? How can we deploy, scale, and monitor such a high number of applications? How can we share and reuse code between services?
