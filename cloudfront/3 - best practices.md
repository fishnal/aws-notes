# CloudFront Best Practices

## Static Assets
- S3 for static data
	- Better if you host a static site on S3 because the data transfer is free between the bucket and distro
	- **Question:** Do you still incur charges for the distro though (most likely??)
- Control access to S3 via Origin Access Identity, which restricts the S3 objects from being accessed only be a CF distro
- Control access to CloudFront via signed URLs or signed cookies
- Do not forward headers, query strings, or cookies to edge caching
- Versioning for objects is useful when you need to do rollbacks

## Dynamic Assets
- Even though the data may change, still useful to cache it because you can still take advantage of If-Modified-Since and If-None-Match for cached objects

**More caching => more availability**
