# Domain Name System (DNS)
A **domain name system (DNS)** is a heirarchical distributed naming system for computers, services, or any resource connected to the internet or a private network
- Associates domain names to IP addresses
- The "phonebook of the internet"

## Route 53
**Route 53** is a highly available and scalable DNS, that **provides secure and reliable routing of requests** both for services in AWS and infrastructure outside of AWS

A **hosted zone** is a container that holds information about how to route traffic for a domain
- A **public hosted zone** determines how traffic is routed on the public internet.
	- Can be created when you register your domain with Route 53
- A **private hosted zone** determines how traffic is routed within a VPC.
	- If the resources are not accessible outside of your VPC, then you can use any domain name you want

**Domain Types**
- **Generic Top-Level Domains (TLDs)** such as `.watch` or `.clothing`
- **Geographic Domains** such as `.au` or `.uk`

### Routing Policies

**Routing policies** - determines how Route 53 will response to these queries
- **Simple Routing Policy** - default policy, for single resources that perform a given function
- **Weighted Routing Policy** - suitable when you have multiple resource records that perform the same function
	- Probability is calculated by weight of individual resource divided by sum of total value in resource record set
- **Failover Routing Policy** - route traffic to different resources based on their health
	- Considered as an **active-passive failover mechanism** because if primary instance is not healthy, then it will route to the secondary instance
- **Geolocation Routing Policy** - based on geographic location
	- If locations overlap (i.e. a country and a continent), then it chooses the smallest denominator (country in this case)
	- Can also be used to restrict access
- **Geoproximity Routing Policy** - based on location between users and the resources
  - Only available when you use [Traffic Flow](./route-53.md#traffic-flow)
- **Latency Routing Policy** - suitable when you have resources in multiple regions and want to lower latency
- **Multivalue Answer Routing Policy** - allows you to get a response from a DNS request from up to 8 records at once that are picked at random (all will be healthy resources)
	- Good for load balancing and high availaibility

# Content Delivery Network (CDN)

## CloudFront
[See CloudFront](/cloudfront/cloudfront.md)

### Distributions
CloudFront uses **distributions** to control which source data it needs to redistribute and to where
- Requires an origin - where the data originates from
	- If using an S3 bucket as origin, then you can create an **origin access identity (OAI)**, which ensures that only this OAI can access and serve content from your bucket. This prevents other people from getting around the CloudFront distirbution and accessing the files directly in the bucket via an object URL
- Select behavior options - defining how you want data at an edge location to be cached
- Configure which edge locations you want to serve content from
- Can associate distribution to a **web application firewall access control list** for more security
- Can specify an SSL certificate for the distribution

**Two types of distributions:**
1. Web Distribution
	- Static and Dynamic content
	- Serve via HTTP/HTTPS
	- Can Add, update, or delete objects
	- Can submit web forms
2. RTMP Distribution
	- Stream media with Adobe Flash media service RTMP protocol
	- Source data for an RTMP distro **must** come from an S3 bucket

## Global Accelerator

The **AWS Global Accelerator** is a global service and is designed to get UDP and TCP traffic from end-users to your resources in AWS faster and more reliably. This is achieved by **utilizing AWS global infrastructure** instead of relying on the public internet

Uses two static IP addresses in order to gain access to your resources
- The IP addresses can be mapped to multiple endpoints, each operating in different regions

Setting up a Global Accelerator
1. Create accelerator and select 2 IP addresses provided by either AWS or yourself
2. Create a listener for TCP or UDP connections
3. Associate listener with an **endpoint group**
	- An endpoint group is associated with a different region
	- There are multiple endpoints within an endpoint group group
	- Can set a **traffic dial** for an endpoint group, which is a percentage of how much traffic should go to this group
	- Can configure health checks
4. Associate and register your endpoints for your application (an ALB, an ELB, an EC2 instance, or an Elastic IP address)
	- Can also assign a weight to route the percentage of traffic to a specific endpoint
