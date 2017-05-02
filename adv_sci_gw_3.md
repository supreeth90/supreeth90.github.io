# Assignment 3
  As part of this assignment, the second part of the class, we started working on solutions for Distributed Data Management.
  

### Problem Statement:
  Microservice Data management is a complicated problem in its own. Each Microservice will have a Database, which has Data that it owns and shared data. The problem here is about maintaining consistency of the data.

### Possible Solutions:
1. Event Driven Approach: Provides Availability and Eventual Consistency.
2. Two phase commit protocol: Provides absolute Consistency.

### Architecture:
  We have an Order Service and a Customer Service with their respective databases. But instead of the Message Broker to communicate the events between the two services, we will talk about the Two-phase commit approach.
  (Photo here)
  
### Progress:
  As part of this assignment, we explored various options for Distributed XA Transaction Managers like Atomikos and Bitronix, connecting with different datasources.
  
  
