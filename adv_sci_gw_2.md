# Assignment 2

### Problem Statement: API Gateway Load Balancing

Our first task is to improve upon the load balancing capabilities of Apache Airavata, API gateway is a part of Airavata's middleware service, providing the essential abstraction and security needed to several micro-services deployed as Component programming interfaces. API gateway is deployed on a single instance in production, hence is the single point of failure for the whole middleware service, therefore this repository will serve as the test bench for trying out all the load balancing and infrastructural technologies and come up with a feasible solution for Airavata.Along with load-balancing, this preoject also serves the infrastructural needs associated with the solution. Please refer Airavata Docs for more details about the architecture.

### Possible solutions:

There are two main areas of concentration here:

1. Software Defined Environment: This part addresses the infrastructural needs for deploying the API-Gateway, or in general defines the next generation environment by adopting DevOps, we tried two different approaches for this. 
  Possible options for Software Defined Environment:
  - Platform dependent Aws centric infrastructure: We setup the whole environment using AWS console, though it served our purpose of fault tolerance, managing this infrastructure was not easy and also this solution was of no value when multiple cloud vendors were in picture.
  - Platform independent Infrastructure: We introduced Terraform (https://www.terraform.io/), a cloud agnostic tool to manage infrastructure for multiple cloud vendors, basically, this solution adds a abstraction layer on top of cloud specific technologies and provides a unified platform to create, change, evolve and destroy infrastructure with ease.

2. Load-Balancing: Our main concern when researching about this topic was to introduce dynamic and automated nature to load-balancing architecture, or to rephrase it, we were more concerned about service discovery and automated configuration of load balancer service. Hence below mentioned are the possible solutions,
  Possible options for Load balancing: 
  - Consul for service discovery and Fabio for load balancing.
  - Consul for service discovery and consul template plus HAProxy for load balancing.

### Evaluation of solution:
  As we are trying to load balance API server here, which runs on Apache Thrift(TCP-based). After our hand-on experience with both of the solutions, we feel that Consul+Consul Template and HA Proxy is a better solution as HA Proxy has better support for TCP load balancing. We observed that fabio has a good support for HTTP load balancing but lacks in TCP load balancing.
  
### More explaination of the Solution:
1. Service Discovery by Consul: This is a service discovery engine running on each node, automatically registers to the network forming a mesh network. We embedded java code which registered load balancers as a service in consul when we started the thrift service.
2. HA Proxy: HAProxy is a free, very fast and reliable solution offering high availability, load balancing, and proxying for TCP and HTTP-based applications.
3. Consul Template: The daemon consul-template queries a Consul cluster and updates any number of specified templates on the file system. We used this feature to update HA proxy.cfg for available API servers.

### Testing:
  We implemented a load testing mechanism using Apache Jmeter: Apache Jmeter doesn't support load testing using TProtocol but provides an option to write an extension called a Sampler. We implemented a Apache Jmeter Sampler to help us with load testing the API server. (link)[https://github.com/airavata-courses/spring17-API-Server/tree/master/mock-airavata-api-load-tester]

