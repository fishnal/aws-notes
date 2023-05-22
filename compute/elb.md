# Elastic Load Balancing

## Elastic Load Balancer (ELB)

To manage and control the flow of inbound requests destined to a group of targets
- Targets could be EC2 instances, Lambdas, IP addresses, containers
- Targets can reside in different AZs, or all placed in a single AZ

ELBs can detect when an instance goes down, and re-route traffic to healthy instances accordingly

Integrates very well and easily with **EC2 Auto Scaling**
- ELB solves the problem of

### Components of ELB

- **Listeners** - defines how inbound connections are routed to your **target groups** based on ports and protocols set as **conditions**
- **Target groups** - a group of resources that you want the ELB to route requests to based on **rules**
- **Targets** - the resources in a target group
- **Rules** - define how an incoming requests gets routed to which target group
- **Health checks** - a health check is performed against all resources within a target group
- **Internet-facing ELB**
	- The **nodes** of an ELB are accessible via internet and thus have a public DNS name, public IP, and an internal IP
	- This allows the ELB to serve incoming requests from the internet
- **Internal ELB** - an internal ELB that has **only** an internal IP, so it can only serve requests that originate from within the ELB's VPC
- **ELB Nodes** - used by ELB to distribute traffic to your target groups
	- Must select which AZs to deploy the ELB nodes to
- **Cross-Zone Load Balancing** - distributes traffic evenly between all resources in all AZs that the ELB is deployed to
	- Suppose you have 4 instances in Zone1 and 10 instances in Zone2
	- Without cross zone balancing, traffic is split equally **amongst the AZs**:
		- 50% of traffic is routed to each zone
		- In Zone1, each of the 4 instances will handle 12.5% of all requests
		- In Zone2, each instance will handle 5% of all requests
	- With cross zone balancing, traffic is split equally **amongst the resources**:
		- Since there are 14 total resources, each resource will end up handling ~7% of all requests.

[**AWS Official Comparison of Elastic Load Balancing**](https://aws.amazon.com/elasticloadbalancing/features/#compare)
- ALB has the most features
- NLB is most performant
- Classic balancer is more of a legacy option, and instead AWS recommends to use ALB or NLB over the classic balancer. _Note: There is an exception, see the Classic Load Balancer section_

[**Differences between ALB and NLB**](https://blog.cloudcraft.co/alb-vs-nlb-which-aws-load-balancer-fits-your-needs/)

![ELB components](./assets/elb-components.png)

## Using HTTPS as an ALB Listener

- You need an X509 server certificate and an associated security policy
	- The certificate is needed to allow the ALB to decrypt the HTTPS requests in order to figure out which target group to forward the request to
- Selecting a certificate:
	- Can select from/upload to ACM (recommended)
	- Can select from/upload to IAM (only recommended when deploying ELBs in regions that are not supported by ACM)

## Application Load Balancer (ALB)

**Operates at Layer 7 of the OSI Model: "Application"**
- Only handles HTTP or HTTPS traffic
- Because it operates at Layer 7, an ALB can route traffic based on any detail in the HTTP request received (i.e. headers, payload, query parameters)
- Provides advanced routing and visibility features
- Example: your ALB may route HTTP traffic to Target Group 1, and HTTPS to Target Group 2
- Some services it offers are HTTP, FTP, SMTP, and NFS

## Network Load Balancer (NLB)

**Operates at Layer 4 of the OSI Model: "Transport"**
- Allows you to balancer requests purely based on the TCP, TLS, or UDP protocol
- Because it operates at Layer 4, an NLB can only route traffic based on the connection
	- Because NLBs do not inspect every aspect of a request, they need significantly less time to forward requests compared to ALBs
- Able to process millions of requests per second while maintaining low latency
- If your application's logic requires a static IP address, then an NLB is a good choice

Supports cross-zone load balancing
- NLB nodes use an algorithm to select a target in a zone
	- Algorithm is based on the TCP sequence, protocol, source port, source IP, destination port, destionation IP
- When a TCP connection is established with a target host, then the connections remains open with that target for the duration of the request

## Classic Load Balancer

- Supports HTTP, HTTPS, and TCP
- **Best practice to use ALB over the classic load balancer**, unless you have an application already running in the EC2-classic network
	- EC2-classic is no longer supported for newer AWS accounts
	- EC2-classic allowed you to deploy instances in a single, flat network shared with other customers, instead of inside a VPC like it's done now

Does not have provide as many features as ALB, HOWEVER has the following features that ALB does not have:
- Support for EC2 classic
- Support for TCP and SSL listeners
- Support for sticky sessions using application-generated cookies
- DOES NOT support target groups, can only route traffic to individual targets

## Gateway Load Balancer (GWLB)

A gateway service for distributing traffic across multiple virtual appliacnces, and also scales them up/down based on demand.

When an instance is unhealthy, GWLB re-routes traffic away from the unhealthy instance in order to maximize app availability and minimize app downtime.
