# Shield

Designed to protect your infra against DDoS attacks
- A **DDoS** attack is when a host receives a large number of requests at the same time from multiple sources

Types of DDoS Attacks
- SYN flood
	- Client sends spoofed SYN packets to initiate a connection with host
	- Host attempts to respond with ACK, but the client never receives it
	- This results in an open connection, which eats up resources
- HTTP Flood
	- Sending large number of HTTP requests
- Cache-Busting
	- Send HTTP requests with query strings that force content to be retrieved from originating server, rather than the cache server
- DNS Query Flood
	- Large amounts of DNS queries can eat up resources on the DNS server, such as Route 53 in AWS

Shield Standard
- Free for everyone
- Offers DDoS protection against common Layer 3 (Network) and Layer 4 (Transport) attacks
- Integrated with CloudFront and Route 53

Shield Advanced
- Additional cost
- Integrates with EC2 and ELB as well
- 24/7 DDoS response team
- Additional monitoring capability, view real-time metrics of attacks
- Offers DDoS protection against Layer 3, Layer 4, and Layer 7 (Application) attacks
- Cost protection - when resources suddenly scale with rise in traffic (that was caused by DDoS attacks)
