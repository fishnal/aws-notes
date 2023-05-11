# Optimizing S3 Performance

## TCP Window Scaling
- Use a window scale to modify the header, wihch allows you to send more data (above the default 64KB) in a single segment
- Window scaling operates at the protocol level, so this can be applied when a client connects to any server
- Brief overview of how a TCP connection is established
	1. Client sends SYN to server
	2. Server responds with ACK, and sends it's own SYN to client
	3. Client responds with ACK to server
	4. Connection is established

## TCP Selective Acknowledgement (SACK)
- Sometimes multiple packets can be lost when using TCP
- One remedy is to resend all packets. However, some of these packets may have already been received by the client, so there would be duplicate packet requests.
- A better option is to use **selective acknowledgement or SACK**. The client sends this to the server so that way only certain packets are sent back.
- SACK must be initiated by the server when the connection between client/server is first established.

## Scaling S3 Request Rates
- Prior to 2018, in order to improve throughput of your bucket, you would randomize the prefixes of your objects
- Since 2018, you no longer have to _randomize_ the prefixes. Instead, you can use normal prefixes as you see fit.
- There is no limit to the number of prefixes that an object has in a bucket.
- Can get 3500 PUT/POST/DELETE requests/s, and 5500 GET requests/s for a single prefix
	- So for 20 prefixes, you could reach 70K PUT/POST/DELETE req/s and 110K GET req/s

## Integration of CloudFront

Pretty straight-forward since this is a caching mechanism hosted on a CDN.
