# Assignment 4
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
 We implemented a sample application to demonstrate 2PC protocol for Distributed Transactions, which was built on Spring Framework with JPA + JTA + Bitronix(Open source DTM). The schema had a Student Table with 2 datasources and we saw the consistency across both the datasources.
 Code: https://github.com/supreeth90/two_phase_commit
 
 ### Advantages of this approach:
 1. Maintains 100% consistency all the time.
 2. Global Transaction Manager knows all the schema and so easy to debug.
 3. Faster because Global Transaction Manager accesses the database directly.
 
 ### Disadvantages:
 1. Global Transaction Manager(GTM) should be access the Database of all the microservices which is a serious violation of microservices architecture.
 2. GTM can be a single point of failure.
 3. If there is a schema change in any microservice, the GTM needs to be updated and restarted.
  
### References:
1. https://dzone.com/articles/xa-transactions-2-phase-commit
2. https://github.com/bitronix/btm

  
