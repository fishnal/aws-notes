# Amazon Kinesis

Designed to address complexity and costs of streaming data into AWS cloud

Kinesis integrates with IAM, KMS (for encrypting data at rest)

## Fundamentals of Stream Processing

Benefits of streaming data
- Streaming reduces need for large and expensive shared databases. This is because every stream consumer maintains its own data and state
- Stream processing fits naturally inside a decoupled microservices architecture
- Can provide actionable insights within _milliseconds_ of a recorded event

### Questions to Consider

How important is it to have immediate insights?
- This deals with the **issue of latency and value of data over time**
- For example, a bug ticketing system probably does not need real-time streams
- However, banks, healthcare, and security services do require real-time or near real-time streaming data (i.e. fraudulent transactions, a hospital patient needs immediate assistance, there's a security breach or vulernability)

### Issues with Batch Processing

Batch jobs usually wait until their workload reaches a certain size (i.e. 100 images to process, 30 videos to process).
- A batch may not run because it only has 99 images, but needs 100 in order to start. As a result, _batches start inconsistently_
- While the size of the batch job is uniform, **time period** is inconsistent

### Challenges of Stream Processing
- Streaming applications are usually "high-touch" systems (lots of human interaction) which make the application inconsistent and difficult to automate
- Difficult to set up - streaming apps usually have lots of brittle components
- Expensive to create, maintain, and scale in on-premises datacenters

## Use Cases

- Clickstream analytics
- Preventative Maintenance
- Fraud Detection
- Emotion analysis
- Dynamic pricing engines

## Data types to stream

Kinesis can stream one of two types of data:
1. Binary encoded data (i.e. audio, video)
2. Base64 text encoded data (i.e. logs, leaderboards, telemetry from devices)

| Component              | Stream Data Type |
| ---------------------- | ---------------- |
| Kinesis Video Streams  | Binary           |
| Kinesis Data Streams   | Text             |
| Kinesis Data Firehose  | Text             |
| Kinesis Data Analytics | Text             |

## Layers of Streaming

1. Source
2. Stream Ingestion - where source data is collected by _producers_, who format data as **"data records"**, and then put them into a stream
	- "Data records" are immutable.
3. Stream Storage - a high-speed buffer that stores data for a minimum of 24 hours, and up to 365 days
	- Data is never removed from storage, it only expires
	- Because data records are immutable, updates to data require a _new_ record to be put into the stream
4. Stream Processing - this layer is managed by consumers, which process data inside the stream
5. Destination - consumers from the "Stream Processing" layer send data records to the final destination. This can be a data warehouse, storage, or even another stream.

## Services in Kinesis

### Kinesis Video Streams

Designed to stream binary-encoded data from millions of sources

Data should be "any type of binary-encoded time-series data"

Data can be sourced from phones, cameras, drones, dash cams, etc.

Supports **webRTC**, which allows for two-way, real-time media streaming between browsers, mobile apps, and connected devices

### Kinesis Data Streams (Very Customizable)

**A Kinesis Data Stream is a Stream Storage Layer**

Highly customizable streaming solution
- Programatically configure data ingestion, monitoring, scaling, elasticity, consumption

AWS provisions resources only when requested

**NOTE: Data Streams does not have the ability to do Auto Scaling. You need to implement this**

AWS provides the **Kinesis Agent for Linux** and **Kinesis Agent for Windows** to help develop and manage Kinesis Data Streams

Components of a data stream
* **Data stream** is a set of **shards**
* **Shard** is a sequence of **data records** of fixed capacity
* **Data record** is immutable structure that contains
	1. Partition key
		- Groups data in a shard in a stream
		- Defines the shard that this record belongs to
		- AWS recommends to use logic that creates random partition keys. By using random partition keys, data records will be distributed across all your shards.
  		- If you used a bad partition key generator, then a lot of data records might be in only 1 of your 3 shards, which means that shard has heavy traffic while the other 2 do not.
	2. Shard ID - which shard it belongs to
	3. Hash key ranges
	4. Sequence number
		- Unique per partition-key within a shard
		- Helps maintain order of arrival within a shard
		- Increase over time for the same partition key (duh...)
	5. Data blob up to 1MB

Data records are available in stream for a finite amount of time
- Ranges from 24 hours (default) to 8760 hours (365 days)
- For extra charge, you can request a 7-day extension for a data record
- Records stored longer than 24 hours but up to 7 days incur an additional charge _per shard hour_
- Records stored longer than 7 days are billed per gigabyte per month (keep in mind the MAX lifetime is 1 year)
  - Can fetch record from stream via `GetRecords`, but this will incur another charge!
- However when using an _"Enhanced Fanout" Consumer_ (see more below), there is no charge for long-term data retrieval if you use `SubscribeToShard`
	| Situation | Billing |
	| --------- | ------- |
	| Stored for 24 hours or less | Standard charge |
	| 7-day extension | Additional charge |
	| After 24 hours and up to 7 days | Billed per shard hour |
	| After 7-days | Data stored in stream billed per GB per month (up to a year)
	| Retrieving data older than 7 days via `GetRecords` | Additional charge |
	| Long-term data retrieval with Enhanced Fanout Consumer via `SubscribeToShard` | No charge |

Shard throughputs
- Shards have a max throughput of 1000 records per second OR 1 MB
- Partition key of a data record is counted as part of throughput limit

Sharp capacities can be on-demand or provisioned

### Types of Consumers

Consumer apps can only be subscribed to 1 stream

**Classic** - pulls data from stream (aka polling mechanism)
* Limit to how many times and how much data consumers can pull per second
* Shard's throughput is shared between all consumers
* Total read throughput of 10 MB/s or 10k records/s
  * Every shard has 2 MB/s read throughput in this config
  * Limit of 5 api calls per second _per shard across ALL consumers_
* After 5 consumers, adding another consumer adds about 1 second of latency
* On average, latency is between 200ms-1000ms

**Enhanced Fan Out** - _subscribes_ to a shard instead of continuously polling
- Costs more than "Classic Consumer"
- Data is pushed automatically from shard to the enhanced consumer
- Shard limits are removed and throughput is NOT shared; _every consumer_ gets 2mbps provisioned throughput per shard
- Consumer subscribes via `SubscribeToShard` and will remain subscribed for 5 minutes
- After 5 minutes, consumer needs to make another `SubscribeToShard` call
- Uses HTTP/2 to increase throughputs, and makes it possible to remove shard limits
- Latency around 70ms
- Soft limit of 20 consumer apps registered per stream

#### HTTP/2 Retrieval

- Major revision to HTTP protocol
- New methods for framing and transporting data
- Designed to reduce latency and increase throughput

### Kinesis Data Firehose

**_Note:_** This is **NOT** the same as Kinesis Data Streams. Keep this in mind!!!

Fully managed, **_near real-time_** streaming delivery service for data

Ingested data can be dynamically transformed, scaled automatically, and automatically delivered to a data store
- Since transformed data is sent to data store, Firehose does not need consumer applications
- Data store can be AWS services, like S3, DDB, or Redshift, or can be 3rd party like Datadog or Splunk
  - For Redshift clusters as the final destination, Firehose will actually upload data to an S3 bucket first, then that is sent to your cluster.
  - For S3 buckets, OpenSearch, and Splunk, you can optionally backup data in a different S3 bucket

Firehose is the main service to Kinesis Data Stream records to an AWS Storage Service

Firehose will buffer incoming data before delivering it to its destination.
* Can configure the buffer size (size of data) and interval (how long buffer should last before sending data to destination)
* Buffer interval ranges from 60s to 900s
  * This makes Firehose "near real-time"

Firehose supports autoscaling

Can convert input data into another format (i.e. JSON to Apache Parquet) OR use your own lambda to convert the data

Pricing
* No free tier
* Costs incurred whenever data is inside a Firehose stream
* Only charged for _used_ capacity, not for provisioned capacity

### Kinesis Data Analytics

Read from stream in real-time, and perform aggregation and analysis while data is in motion

Can leverage SQL queries or Apache Flink to perform time-series analytics, feed real-time dashboards, and create real-time metrics
* When using Kinesis Data Firehose with Kinesis Data Analytics, data records can only be queried using SQL
* Apache Flink can only be used with Kinesis Data Streams

Has built-in templates for organizing, transforming, aggregating, and analyzing data

Use cases
* ETL - Extract, Transform, Load
	* Goal is to aggregate data in one format in a warehouse
* Continuous Metrics
* Responsive, real-time analytics (i.e. triggering alarms, sending notifications)

## Pricing Factors

Video Stream
* Volume of data ingested
* Volume of data consumed
* Data stored across all video streams for a given account

Data Stream
* Hourly cost based on # of shards in stream (regardless of whether data is actually in the stream)
* Producers putting data into stream storage
* Optional: when extended data retention is enabled, extra hourly charge per shard
* Only for Enhanced Fan Out consumers: based on amount of data and # of consumers

Firehose
* Amount of data put into delivery stream
* Amount of data converted
* If data is sent to a VPC, then incur additional charges for the amount of data and hourly charge per AZ

Analytics
* Hourly rate based on # of **Amazon KPUs** used
* **KPU - Kinesis Processing Unit**, which is 1 virtual CPU and 4GB of memory
