# Data in Kinesis Data Streams

Recall that Kinesis Data Stream is a **storage layer for stream processing**

## Quick Comparison of Data Streams vs Data Firehose

| Data Streams                                                   | Firehose                           |
| -------------------------------------------------------------- | ---------------------------------- |
| Requires custom code to get data from stream                   | Does not require custom code       |
| Can access data with multiple consumer apps that are different | Cannot create custom consumer apps |

## Writing Data to the Stream

Can invoke `PutRecord` and `PutRecords`

`PutRecord`
- Requires stream name, partition key, and data

`PutRecords`
- Requires stream name and data, however partition keys are included in the data
- Returns a response that contains number of failed records -- use this for resiliency
  - This is likely to happen because of throughput exceeded exceptions
- Prefered to the singular version `PutRecord`
  - Less HTTP overhead (i.e. initiating connection or TLS handshakes)
  - Better for app performance (making 3k small requests vs 30 batched requests)

## Consuming Data from the Stream

Each thread gets data via `GetRecords` and `GetShardIterator`

`GetShardIterator`
- Returns location of the first data record available in the stream
- Can be a sequence number, timestamp, the first record in the stream, or the most recent record
- 5 possible starting positions
  - `AT_SEQUENCE_NUMBER` - gives stream status, but no details about shards
  - `AFTER_SEQUENCE_NUMBER` - start reading after the position specified
  - `AT_TIMESTAMP` - start reading from stream at specified time
  - `TRIM_HORIZONG` - starts at oldest record in shard
  - `LATEST` starts with most recent record put into shard

`GetRecords`
- Returns data records and **a value called `NextShardIterator`**
  - Consumers should continue polling based on this value. If it is null, then the _shard is closed_ and the iterator will not return any more data
- Max number of records returned is 10,000
- Max amount of data returned is 10MB

If a consumer received a throughput exceeded exception, then it should backoff and retry later

## Network Latency

To minimize network latencies for producers inside a VPC, always use an **Interface VPC Endpoint**.
- Keeps traffic between VPC and Kinesis Data Streams inside the Amazon network, which minimizes latency
- Reduces the risk of your data being exposed elsewhere on the internet

**Backpressure** refers to when throughput is met with resistance (i.e. bottlenecks)

## Kinesis Producer Library (KPL)

Designed ONLY for Kinesis Data Streams

KPL helps with putting records into a stream.

Why?
- Keep in mind that a shard is billed by the hour
- Shard suppots up to 1k records/s at a max rate of 1 MB/s
- Realistically however, streaming data is usually made up of much smaller records
  - For example, tracking login attempts which might be 50-100 _bytes_ in size
- As a result, you are not using all of your shard's throughput, and thus wasting money!

Utilizing an SDK with these points in mind can be wasteful. **KPL** solves this problem by attempting to use as much throughput as possible

Kinesis Firehose can actually use a Kinesis Data Stream as a producer. To achieve this,
1. Agreggate data records and put them into a Data Stream
2. Then have Firehose use the Data Stream as a producer
3. If the records were inserted via KPL, then Firehose will detect that and can unpack it before sending the records to their destination

KPL has retry-logic built in

## Kinesis Client Library (KCL)

Designed to be an intermediary between record processing logic and Kinesis Data Streams

Sister library of KPL
- KPL aggregates data, KCL de-aggregates data

Other features
- Load balancing across multiple consumers
- Responds to consumer instance failures by [checkpointing records](#checkpointing-records)
- Reacts to resharding operations

### Checkpointing Records

Allows a consumer to resume progress when an app goes down

Checkpoints are **implemented using DynamoDB** to track a **sub-sequence number**, which is related to a _data record's sequence number_

There is 1 row for each shard in the stream
- Because of this, DDB can throttle if it does not have enough RCUs/WCUs. Consequently, your stream will also experience throttling
- Best fix for this issue is on-demand provisioning

## Security for Data Streams

Can encrypt data in transit via HTTPS and at rest via KMS

FIPS endpoints are available if you require `FIPS 140-2` encryption

## Data Streams and VPCs
