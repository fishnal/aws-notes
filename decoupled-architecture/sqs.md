# Simple Queue Service (SQS)

Handles delivery of messages between components

Supports SSE with KMS

Flow
- Follows a **send/receive** model
- Producers push messages to queue
- Queue is replicated across multiple servers for resiliency
- When consumers are reaady, consumer polls for a message and the queue starts a **visibility timeout**

## Visibility Timeout
- When a message is retrieved by a consumer, visibility timeout is started
	- Default visibility timeout is 30s, but ranges from 0s to 12h
	- If a consumer needs more than 12h to process a message, consider using Step Functions instead
- During this window, the message is **not visible** to other consumers
- During this timeout period, the message can only be deleted if the consumer sends the `DeleteMessage` request.
	- Useful in case the consumer fails to process the message and does not send the "delete message" request
- Once the timeout expires, the message is available to other consumers
	- **Note:** The visibility timeout should be longer than it takes for a consumer to process a message
- **How can the timeout expire?**
	- If the consumer does not send `DeleteMessage` within the timeout window (i.e. a computation error is thrown so it never reaches this action)
	- The actual message is corrupted. If this happens, then this message can be stuck in an infinite loop where it gets received by a consumer but never deleted from queue. To avoid this, setup a [**Dead Letter Queue**](#dead-letter-queue-dlq)

## Size Constraints
- Minimum message size is 1 byte
- Maximum message size is 256KB (default)
- Can use SQS Extended Client libraries, which allow you to send much larger messages (up to 2GB).
	- Instead of putting the entire payload in the message, the library will upload the payload to an object in S3
	- Then the SQS message is just a link to that object

## Queue Types

### Standard Queues
- **"At least once"** delivery of messages
	- A producer could send the same message multiple times to the queue
- Messages may appear out of order
- Standard Queue offers "best effort" to preserve message ordering
- Provides almost unlimited number of transactions per second (tps)
- Maximum of 120,000 **in-flight messages**, which are messages that have been received by a consumer but not yet deleted (i.e. consumer has not finished processing the message)

### FIFO Queues
- First-in-First-out (preserves message ordering)
- **Exactly once processing**
- Limited TPS (300 tps)
- Can send up to 10 messages in a batch
- Maximum of 20,000 **in-flight messages**

### Dead Letter Queue (DLQ)
- DLQs are associated with each SQS queue
- If a message can't be processed by a consumer after a max number of tries, the queue sends the messages to DLQ
- Can break the order of processing in FIFO queues
- Enabled for an SQS queue by enabling the **"Redrive Policy"**, which specifies
	- The **source queue** - from where the failed messages come from
	- The **dead letter queue** to send failed messages to
	- Conditions that define if the message should be moved to DLQ, such as max number of attempts
	- Which source queues can _access_ the DLQ
- If a queue is FIFO, then it's corresponding DLQ must also be FIFO

## Polling Types

Consumers consume messages by sending a `ReceiveMessage` request
- Response can return no messages
- Can specify `MaxNumberOfMessages` in request

Two types of ways to poll:
1. **Short Polling** (default) occurs when either
	- `WaitTimeSeconds` in request is 0 or
	- `ReceiveMessageWaitTimeSeconds` is 0
2. **Long Polling** occurs when either
	- `WaitTimeSeconds` is `>` 0 and `<=` 20 seconds
	- `ReceiveMessageWaitTimeSeconds` is greater than 0

When responses are received:
- Short polling: immediately (even if there are no messages).
- Long polling: SQS will return messages (if any) **after** the specified **wait time**

Quota Concerns
- Short polling can cause your consumer to exceed the **quota of in-flight messages**, and you can get an `OverLimit` error
- Long polling does not throw errors such as `OverLimit`
- Always make sure to delete messages when you are done with them
