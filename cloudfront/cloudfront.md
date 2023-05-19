# CloudFront

CloudFront (CF) is a Content Delivery Network (CDN) that speeds up distribution of static and dynamic content via **edge locations.**
- Edge locations are deployed in major cities and highly populated areas

Also take a look at [Global Infrastructure](/cloud-practitioner/global-infrastructure.md)
- Origin server sits behind **AWS Origin Shield**
- **AWS Origin Shield** will distribute content to **regional edge caches**
- Regional edge caches then distribute to **edge locations**

## Basic Flow

1. When a user requests content from a CF distrobution, the request gets forwarded to the edge location with **least latency**.
2. If the edge the content cached, it will deliver the cache instead. Otherwise, the edge will fetch the content from the origin servers.
	- Cached objects will eventually expire (a value you can set)

## Cached Objects

As mentioned before, cached objects eventually expire. By default, they expire after 24 hours. This expiration date is also known as a **Time-To-Live (TTL)**.

When an cached object expires on an edge, the cached object is **not** deleted. This is because the cached object also contains metadata about the object, such as the *version* of the object.

This version is then compared against the object's version on the *origin server*. If the versions differ, then the edge will request the new version from the origin server. Otherwise, the edge will continue using it's cached object and refresh it's expire timer.

## Invalidation Requests

An **invalidation request** removes **every** cached version of a file from **all** edge locations.

You can invalidate a specific file and files that match a given pattern.

## Reports

* Cache Statistics - gives information related to edge locations for last 60 days
* Popular Objects - lists 50 most popular objects and stats about these objects (i.e. # of requests, hits, misses, repeat downloads, etc)
* Top Referrers - lists top 25 referrers
* Usage - information about number of requests, data transferred, etc.
* Viewers - information about type of device, browser, and OS being used by your users, and location of your users

## Patterns/Use-Cases

#### To cache and secure content with an ALB as the origin

Assumptions
- ALB is internet facing
- ALB routes traffic to EC2 instances

Normal setup: users hit your ALB and request is forwarded to the respective targets

CloudFront setup: users hit your CF distrobution for cached results, which uses the ALB as the origin server
- Must setup a Route 53 record: ALB's domain name is an **alias** for your distrobution's domain name
- Since ALB is public, if someone knows the DNS name of your load balancer, then they can directly connect to the load balancer, bypassing the benefits of your CF distro
- To secure this, do the following
	1. Configure your distro to add a custom header (when it makes requests to the ALB/origin server).
	2. Set your distro's **origin protocol policy** to HTTPS. This will ensure your requests to the origin server are encrypted, so your custom secret header is not leaked
		- Make sure to rotate this custom header regularly
	2. Configure your ALB to forward requests that have this custom header. If they don't, drop them or send a 403.

#### To cache and secure content with S3 bucket as the origin

Useful for
- General caching of objects in S3 bucket
- Static website hosting

However, static website hosting on S3 buckets has limited features
- Does not support HTTPS
- Does not support caching
- Latencies are bad for users that are far from the bucket's region/location
- Requires you to make the entire bucket available for **public read access**

CloudFront distros solve this problem
- Can configure HTTPS
- Can configure a friendly domain name (which also requires an SSL cert for that domain name)
- Distributes content globally, so users around the world have better latencies
- Can remove public access to the S3 bucket and only grant your CF distro access to the S3 bucket. This is done via **Origin Access Identities (OAI)** and **S3 bucket policies**
	1. Create and associate an OAI for your distro
	2. Can then automatically update the bucket policy, or you can manually edit.
		- Principal should be OAI's ARN
		- Should only need `s3:GetObject`
	3. Disable public access to your bucket
