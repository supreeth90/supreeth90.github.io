# Assignment 5
  As part of this assignment, we started working on solutions for Distributed Data Management.
  
### Problem Statement:
  Microservice Data management is a complicated problem in its own. Each Microservice will have a Database, which has Data that it owns and shared data. The problem here is about maintaining consistency of the data.

### Possible Solutions:
1. Event Driven Approach: Provides Availability and Eventual Consistency.[Event-driven](https://en.wikipedia.org/wiki/Event-driven_architecture)
2. Two phase commit protocol: Provides absolute Consistency.[2PC](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)

### Architecture:
  We have an Order Service and a Customer Service with their respective databases. But instead of the Message Broker to communicate the events between the two services, we will talk about the Two-phase commit approach.
![Image](http://www.yusufaytas.com/wp-content/uploads/2012/10/2PhaseCommit.png)
 
### Phases in 2PC:
  1. Prepare Phase: The initiating node, called the global coordinator, asks participating nodes other than the commit point site to promise to commit or roll back the transaction, even if there is a failure. If any node cannot prepare, the transaction is rolled back.
  2. Commit Phase: If all participants respond to the coordinator that they are prepared, then the coordinator asks the commit point site to commit. After it commits, the coordinator asks all other nodes to commit the transaction.
  
### Progress:
  During our previous assignment, we worked on a solution based on Global Transaction Mananger, which was criticized for violating some of the core principles of Microservices Architecture.
  Solution 1.0: We implemented a sample application to demonstrate 2PC protocol for Distributed Transactions, which was built on Spring Framework with JPA + JTA + Bitronix(Open source DTM). The schema had a Student Table with 2 datasources and we saw the consistency across both the datasources.
 Code: https://github.com/supreeth90/two_phase_commit

### Solution 2.0:
  To address some of the issues found in Solution 1.0, we implemented an architecture which doesn't have GTM instead it relies on lot of Master-slave communications which can provides much more flexibilty. 
  There will be Master Service for each schema, which owns the schema, and it is the responsibility of this service to maintain consistency of data across all the microservices which uses this schema. Essentially, the master service is per schema basis and there can be many master microservices.  
 
 ### Advantages of this approach:
 1. Maintains 100% consistency all the time.
 2. Master service doesn't have direct access to other databases. 
 3. Works sychronously. 
 
 ### Disadvantages:
 1. Can be slow due to its synchronous nature.
 2. Not fault tolerant in a node goes down.
 3. Need a lot of synchronisations in case of schema updates and availability might take a hit.
  
### References:
1. https://dzone.com/articles/xa-transactions-2-phase-commit
2. https://github.com/bitronix/btm

  
