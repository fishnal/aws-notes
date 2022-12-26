# CloudFront

* Acts as a **Content Delivery Network (CDN)**
* Stores cached data for faster access
    * Because data is cached, data i not durable...
* Distributes web traffic closer to the end user via **edge locations**
* Origin of data can be from an S3 bucket

## Edge Locations
* Edge locations are sites in highly populated areas
* They are **not** used for deploying infrastructure
* Allow ability to cache data and reduce latency for end-users
* Users are redirected to the closest edge location to minimize latency for users

## Distrobutions
* Web Distrobution
* RTMP Distrobution

**Web distrobutions** can distribute static or dynamic content
* Uses HTTP and HTTPS
* Can add, remove, update objects
* Can provide live stream functionality to the website
* Uses an "origin" to define where the source data is (could be EC2 instance, S3 bucket)

**RTMP distrobutions** are designed for distributing streaming media via the Adobe Flash media server RTMP protocol
* Allows user to start viewing media before it's fully downloaded
* Source data can only come from S3 bucket

## Distrobution Configuration
A CloudFront distrobution can be configured to specify
* Origin location of data
* Caching options
* Which edge locations you want your data to be distributed to

# WAF (Web Application Firewall)

- Provides additional security for web application tier
- Encryption applied through SSL certs

## High-Level Process
1. User requests data from a website
2. If the requested content is dynamic, then content is fetched directly from the origin servers. Otherwise, user is redirected to nearest edge location
3. If content is present at this edge, then content is served from the edge instead of the data's origin (this is way faster!).
    * Otherwise, the CloudFront distrobution will fetch and cache the content from the origin, and then it will send it back to the user
