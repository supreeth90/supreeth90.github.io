# Advanced Science Gateway Architectures: Final Report

## Problem Statements
1. API server Load-balancing and High Availability: To improve the load balancing capabilities of Apache Airavata, API gateway is a part of Airavata’s middleware service, 
providing the essential abstraction and security needed to several micro-services deployed as CPI. API gateway is deployed on a single instance in production, 
hence is the single point of failure for the whole middleware service, therefore this repository will serve as the test bench for trying out all the load balancing and infrastructural technologies and to decide upon
a feasible solution for Airavata.
2. Microservices data management: Microservice Data management is a complicated problem in its own. Each Microservice will have a Database, which has Data that it owns and shared data. 
The problem here is about maintaining consistency of the data.

## Problem 1: API server Load-balancing and High Availability:
### Possible solutions for API server Load-balancing and High Availability:
  1. Consul for service discovery and Fabio for load balancing.
  2. Consul for service discovery and consul template plus HAProxy for load balancing.
  
### Evaluation of approches for API server Load-balancing and High Availability:
As we are trying to load balance API server here, which runs on Apache Thrift(TCP-based). After our hand-on experience with both of the solutions, 
we feel that Consul+Consul Template and HA Proxy is a better solution as HA Proxy has better support for TCP load balancing. We observed that fabio 
has a good support for HTTP load balancing but lacks in TCP load balancing.

### Conclusions and recommendations for API server Load-balancing and High Availability:
Service Discovery by Consul: This is a service discovery engine running on each node, automatically registers to the network forming a mesh network. We embedded java code which registered load balancers as a service in consul when we started the thrift service.
HA Proxy: HAProxy is a free, very fast and reliable solution offering high availability, load balancing, and proxying for TCP and HTTP-based applications.
Consul Template: The daemon consul-template queries a Consul cluster and updates any number of specified templates on the file system. We used this feature to update HA proxy.cfg for available API servers.

## Problem 2: Microservices data management:
### Possible solutions for Microservices data management:
1. Event Driven Approach: Provides Availability and Eventual Consistency.[Event-driven](https://en.wikipedia.org/wiki/Event-driven_architecture)
2. Two phase commit protocol: Provides absolute Consistency.[2PC](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)

### Phases in 2PC:
1. Prepare Phase: The initiating node, called the global coordinator, asks participating nodes other than the commit point site to promise to commit or roll back the transaction, even if there is a failure. If any node cannot prepare, the transaction is rolled back.
2. Commit Phase: If all participants respond to the coordinator that they are prepared, then the coordinator asks the commit point site to commit. After it commits, the coordinator asks all other nodes to commit the transaction.
  
### Approaches:
1. Using Distributed XA Transactions with Global Transaction Manager: We implemented a sample application to demonstrate 2PC protocol for Distributed Transactions, which was built on Spring Framework with JPA + JTA + Bitronix(Open source DTM). The schema had a Student Table with 2 datasources and we saw the consistency across both the datasources.
2. Synchronous approach without Global Transaction Manager: To address some of the issues found in Solution 1.0, we implemented an architecture which doesn't have GTM instead it relies on lot of Master-slave communications which can provides much more flexibilty. There will be Master Service for each schema, which owns the schema, and it is the responsibility of this service to maintain consistency of data across all the microservices which uses this schema. Essentially, the master service is per schema basis and there can be many master microservices.  
  
### Evaluation of approches for Microservices data management: 
Implementation 1, though it maintains consistency, has some serious disadvantages such as Global Transaction Manager(GTM) should be access the Database of all the microservices which is a serious violation of microservices architecture.ALso, GTM can be a single point of failure.And, if there is a schema change in any microservice, the GTM needs to be updated and restarted. In order to improvise on some of these drawbacks of this Implementation, Implementation 2 was tried out. During initial testing with 2 microservices(Customer and Order service), the results were promising with consitency being maintained and the advantage of master service doesn't need to have direct access to other databases is good. 

### Conclusions and recommendations for Microservices data management:
As stated by CAP theorem, both the approaches, Event-driven and two phase commit guarantees either Availability or Consistency. So, if an application is mission critical and cannot compromise on the consistency, then the obvious choice is to go with distributed transactions with two phase commit. Also, Bitronix or Atomikos is a better choice as they are production ready and have good support though they have serious limitations for microservices and schema changes. But, for applications with good availability needs, event-driven approach is more suitable as it provides eventual consistency without compromise on availability.
