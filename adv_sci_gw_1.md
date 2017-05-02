# Assignment 1

###Problem Statement: API Gateway Load Balancing

Our first task is to improve upon the load balancing capabilities of Apache Airavata, API gateway is a part of Airavata's middleware service, providing the essential abstraction and security needed to several micro-services deployed as Component programming interfaces. API gateway is deployed on a single instance in production, hence is the single point of failure for the whole middleware service, therefore this repository will serve as the test bench for trying out all the load balancing and infrastructural technologies and come up with a feasible solution for Airavata.Along with load-balancing, this preoject also serves the infrastructural needs associated with the solution. Please refer Airavata Docs for more details about the architecture.

###Possible solutions:

There are two main areas of work here:

Software Defined Environment: This part addresses the infrastructural needs for deploying the API-Gateway, or in general defines the next generation environment by adopting DevOps, we tried two different approaches for this.

Aws centric infrastructure: We setup the whole environment using AWS console, though it served our purpose of fault tolerance, managing this infrastructure was not easy and also this solution was of no value when multiple cloud vendors were in picture.
The last solution was criticized for being heavily dependent on AWS, hence this time around, we have introduced Terraform (https://www.terraform.io/), a cloud agnostic tool to manage infrastructure for multiple cloud vendors, basically, this solution adds a abstraction layer on top of cloud specific technologies and provides a unified platform to create, change, evolve and destroy infrastructure with ease.
Load-Balancing: Our main concern when researching about this topic was to introduce dynamic and automated nature to load-balancing architecture, or to rephrase it, we were more concerned about service discovery and automated configuration of load balancer service. Hence below mentioned are the possible solutions,

Consul for service discovery and Fabio for load balancing, this Blog explains in brief about this solution.
Consul for service discovery and consul template plus HAProxy for load balancing.
