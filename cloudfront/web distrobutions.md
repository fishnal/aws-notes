# Web Distrobutions

A CloudFront web distrobution allows you to serve static or dynamic content.

## Origins

Each distrobution has an **origin**, which is where content is originally fetched from.
* A distrobution can have multiple origins

An origin can point to an S3 bucket or an HTTP/HTTPS server
- If you are using an S3 bucket as a static site, then you need to use the static website endpoint of that bucket.

## Parts of an Origin

The **origin domain name** is the DNS name of your origin.

You can optionally specify an **origin path**, which is a directory in your origin. The origin path will be appended to the origin domain name.

If your origin is an S3 bucket, you can use an **origin access identity** to restrict access to the bucket.
* These identities are CloudFront-specific
* This will not remove any existing permissions

## Cache Behavior

* Can specify certain requests/path patterns to be forwarded to the origin
	* By default, the distrobution will forward *all* requests to your origin
	* Note: Keep in mind that "edges" and "distrobutions" are different. Edges maintain cached content, while the distrobution manages and forwards requests to your origin as necessary
* Can specify what protocol (HTTP or HTTPS) viewers must use to connect to your edges.
* Can specify what HTTP methods your distrobution will process and forward to your origin
	* `GET` and `HEAD` requests will always be cached.
* Can forward headers to origin *and* cache objects based off of request headers.
* Can specify how long a cached object is valid for (TTL)
* Can forward cookies to origin
* Can forward query strings to origin
* Can restrict viewers to use signed URLs or signed cookies

## Distrobution Settings
* Price Class
* Alternate Domain Names (CNAMEs)
* SSL Certificate (either one provided by cloudfront, or a custom one)
* Default root object - by setting one, you avoid exposing the contents of your distro
* Enable logging, and the bucket where logs are stored
* Distrobution state - enabled/disabled (just a toggle for the distro, in case you don't want people interacting with it the moment the distro is created)
