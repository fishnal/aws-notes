# Simple Queue Service (SQS)

Handles delivery of messages between components

Supports SSE with KMS

Flow
- Producers push messages to queue
- Queue is replicated across multiple servers for resiliency
- When consumers are reaady, queue sends message to a consumer and starts a **visibility timeout**

**Visibility Timeout**
- When a message is retrieved by a consumer, visibility timeout is started
	- Default time is 30s, but can be as long as 12h
- During this window, the message is **not visible** to other consumers
- During this timeout period, the message can only be deleted if the consumer sends the "delete message" request.
	- Useful in case the consumer fails to process the message and does not send the "delete message" request
- Once the timeout expires, the message is available to other consumers
- **Note:** The visibility timeout should be longer than it takes for a consumer to process a message

## Queue Types

Standard Queues
- "At least once" delivery of messages
	- A producer could send the same message multiple times to the queue
- Messages may appear out of order
- Standard Queue offers "best effort" to preserve message ordering
- Provides almost unlimited number of transactions per second (tps)

FIFO Queues
- First-in-First-out (preserves message ordering)
- Limited TPS (300 tps)
- Can send up to 10 messages in a batch
- Exactly once processing

Dead Letter Queue (DLQ)
- DLQs are associated with each SQS queue
- Enabled for an SQS queue by enabling the "Redrive Policy"
- If a message can't be processed by a consumer after a max number of tries, the queue sends the messages to DLQ
