# Simple Notification Service (SNS)

**Publish/Susbcribe Model**
- Messages are published in **topics**, which is a group of messages
- When messages are published, ALL subscribers in that topic are **notified**
- Message are NOT persisted; once the message is published, only current active subscribers can receive the message

A service can subscribe via
- HTTP/HTTPS
- Email
- Email-JSON
- SQS
- Applications
- Lambda
- SMS

Topics **can restrict what types of subscribers are allowed** (i.e. a topic may only allow SMS subscribers)

Policies for topics follow same format as IAM policies

## Size Constraints
- Similar to [SQS size constraints](./sqs.md#size-constraints)
- Maximum message size of 256KB
- SNS breaks messages into 64KB chunks
- Can also use SNS Extended Client libraries
	- Allows you to publish messages up to 2GB
	- Achieved through the same approach that SQS Extended Client libraries do it

**Message Filtering**
- Subscribers can filter out what they want to subscribe to
- Filtering is based on attributes of the message

**Retry Options**
- Your SNS's **delivery policy** is used to control the retry pattern for your messages (i.e. linear, geometric, exponential backoff, min/max retry delays)
- When this policy is exhausted, messages are moved to the DLQ
