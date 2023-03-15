# Decoupled Architecture
**Decoupling your components** means
- Each component performs its tasks independently
- Changing one component does not affect others
- Failure in one component is isolated away from others

**Event-Driven Architecture
**- **Producer** - entity that pushes an event to event router
- **Event router** - processes events and takes actions to push outcomes to consumers
- **Consumer** - consumes an event from event router

## SQS
[See SQS](/sqs-sns-ses/sqs.md)
- **Active delivery** via **polling**
- Messages are persisted
- **Visibility timeout** and how it relates to `DeleteMessage` action
- Short polling vs Long polling (see below)
- Performance difference between Standard and FIFO queues
- Dead Letter Queues

## SNS
[See SNS](/sqs-sns-ses/sns.md)
- **Passive delivery** via **pushes and subscriptions**
- Messages are NOT persisted

## Amazon MQ

**Amazon MQ** is a managed message-broker service for Apache ActiveMQ
- Compliant with JMS, NMS, MQTT, and WebSockets
- Highly available
- Storage is implemented across multiple AZs
- Can implement active and standby configurations with automatic failover
- Encrypts data in transit via SSL
- Encrypts data at rest using AES-256
- Integrates with CloudWatch and CloudTrail

## Kinesis Data Streams for Real-Time Processing

**Kinesis Data Streams** is a real-time data collection messaging service, built to handle large throughput volumes
- Maintains a copy of all data received over a given time period, also known as a **shard**; ranges from 24h (default) to 8760 hours (365 days)
- Message ordering is guaranteed **but only within a shard**
- Components of a data stream
	- Shards can write 1000 records per second, up to 1MB per second
	- Shards can reads 5 records per second, up to 2MB per second
	- Data capacity of a stream is proprotional to the # of shards
- Consumers process data in stream via the **Kinesis Client Library**
	- KCL differs from the Kinesis Data Streams API because KCL ensures that every shard has its own record processor running and processing that shard
	- The record processors simplify reading data from a stream and decouple your record procesing logic from "reading data from stream" logic
	- **KCL uses a DynamoDB table to store data**, and creates one table per application processing the stream
- Can automatically encrypt data (by leveraging KMS) as producers deliver data
- **Put-To-Get Latency** is less than 1 second, which is time it takes for producer to deliver data to stream and have a consumer read that data
- On demand capacity vs Provisioned capacity

See the following
- [Kinesis - Layers of Streaming](/kinesis.md#layers-of-streaming)
- [Kinesis - Data Streams](/kinesis.md#kinesis-data-streams-very-customizable)

## EventBridge

**EventBridge** is a serverless **event bus**
- An **event bus** is like an event-coordinator
- Can take in information from external SaaS providers, other AWS services, or custom apps
- EventBridge will filter and direct events to other systems and take action based on certain events

## Step Functions
[See Step Functions](/step-functions.md)

## API Gateway
[See API Gateway](/api-gateway.md)

## AppSync

AppSync allows you to manage and synchronize data across multiple devices and users
- Uses GraphQL to allow clients to fetch, change, and subscribe to data all from a single endpoint
