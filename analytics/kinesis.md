# Amazon Kinesis

Designed to address complexity and costs of streaming data into AWS cloud

Kinesis integrates with IAM, KMS (for encrypting data at rest)

## Fundamentals

Benefits of streaming data in real-time or near real-time
- Streaming reduces need for large and expensive shared databases. This is because every stream consumer maintains its own data and state
- Stream processing fits naturally inside a decoupled microservices architecture
- Can provide actionable insights within _milliseconds_ of a recorded event

Questions to consider when deciding whether to use stream processing
- How important is it to have immediate insights?
	- For example, a bug ticketing system probably does not need real-time streams
	- However, banks, healthcare, and security services do require real-time or near real-time streaming data (i.e. fraudulent transactions, a hospital patient needs immediate assistance, there's a security breach or vulernability)

Challenges of Stream Processing
- Streaming applications are usually "high-touch" systems (lots of human interaction) which make the application inconsistent and difficult to automate
- Difficult to set up - streaming apps usually have lots of brittle components
- Expensive to create, maintain, and scale in on-premises datacenters

## Data types to stream

Kinesis can stream one of two types of data:
1. Binary encoded data (i.e. audio, video)
2. Base64 text encoded data (i.e. logs, leaderboards, telemetry from devices)
|Component|Stream Data Type|
|---------|----------------|
|Kinesis Video Streams|Binary|
|Kinesis Data Streams|Text|
|Kinesis Data Firehose|Text|
|Kinesis Data Analytics|Text|

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

### Kinesis Data Streams (Very Customizable)

**A Kinesis Data Stream is a Stream Storage Layer**

Highly customizable streaming solution
- Can programatically configure data ingestion, monitoring, scaling, elasticity, consumption

AWS provisions resources only when requested

**NOTE: Data Streams does not have the ability to do Auto Scaling. You need to implement this**

Components of a data stream
* A **data stream** is a set of **shards**
* A **shard** is a sequence of **data records** of fixed capacity
* A **data record** is immutable structure that contains
	1. Partition key
		- Groups data in a shard in a stream
		- Defines the shard that this record belongs to
	2. Sequence number
		- Unique per partition-key within a shard
		- Helps maintain order of arrival within a shard
		- Increase over time for the same partition key (duh...)
	3. Data blob (up to 1MB)

For data older than 7 days, can get from the stream via `GetRecords`, but this incurs charges.
* However when using an "Enhanced Fanout" Consumer (see more below), there is no charge for long-term data retrieval if you use `SubscribeToShard`

Types of consumers
* Classic - pulls data from stream (aka polling mechanism)
* Enhanced Fan Out - consumer _subscribes_ to a shard instead of continuously polling
	- Data is pushed automatically from shard to the enhanced consumer
	- Shard limits are removed; every consumer gets 2mbps provisioned throughput per shard

### Kinesis Data Firehose

**_Note:_** This is **NOT** the same as Kinesis Data Streams. Keep this in mind!!!

Fully managed, **_near real-time_** streaming delivery service for data

Ingested data can be dynamically transformed, scaled automatically, and automatically delivered to a data store

Firehose does not need consumer applications

Firehose will buffer incoming data before delivering it to its destination.
* Can configure the buffer size (size of data) and interval (how long buffer should last before sending data to destination)
* Buffer interval ranges from 60s to 900s

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
* Producerts putting data into stream storage
* Optional: when extended data retention is enabled, extra hourly charge per shard
* Only for Enhanced Fan Out consumers: based on amount of data and # of consumers

Firehose
* Amount of data put into delivery stream
* Amount of data converted
* If data is sent to a VPC, then incur additional charges for the amount of data and hourly charge per AZ

Analytics
* Hourly rate based on # of Amazon KPUs used
* KPU - Kinesis Processing Unit, which is 1 virtual CPU and 4GB of memory
