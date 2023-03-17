# CloudFront

CloudFront (CF) is a Content Delivery Network (CDN) that speeds up distribution of static and dynamic content via **edge locations.**
- Edge locations are deployed in major cities and highly populated areas

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
