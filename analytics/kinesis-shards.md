# Kinesis Shard Capacity and Scaling

Kinesis Data Streams are elastic, **but it is not automatic or managed by AWS**
- As a result, Data Streams do not support auto scaling

Shards can be **hot** or **cold**
- Hot Shards are ingesting a large throughput of data
- Cold Shards are not ingesting much

Hot shards should be **split**; cold shards should be **merged**

## Shard States

Shards move into various states when splitting or merging.

**Open**
- Data records can be both added to and retrieved from the shard

**Closed**
- Data records are still available until it expires
- No new data can be ingested, but data can still be consumed
- Instead, new data records coming into the parent are re-routed to the children

**Expired**
- The shard's retention period has expired, and you can no longer consume data from this shard

Situations to Consider
- AWS will NOT tell consumers to prioritize consuming from a parent shard over a child shard. **This has to be managed by you**
- If the order of data records matter, then you have to make sure you consume from parent shards first, then read from child shards once all data in the parent shard has been consumed.

## `UpdateShardCount`

Stream-level API call that updates the shard count of a Kinesis Data Stream
- Asynchronous call that can happen while stream is actively used

Temporary shards are created, and count towards total shard limit for the account
- Each AWS account has a soft limit of 500 shards for US East, US West, and Europe
- Other regions have soft limit of 200

Choose a multiple of 25% when deciding to split or merge shards
- More efficient than other target percentages

Limitations
- Can increase shard count up to double of the current shard count (splits makes 1 shard into 2)
- Can decrease shard count down to half of the current shard count (merges combine 2 shards into 1)

## Scaling Limtations

- Only 1 split or merge can happen at once
- Requests can take some time to complete
  - i.e. Doubling shards from 1000 to 2000 can take more than 8 hours to complete
  - Recommended to scale your shards well ahead of time

Although auto scaling is not a feature, you can implement it (programmatically) via Lambdas with CloudWatch and AWS Application Auto Scaling

## Provisioning

2 values that determine how many shards you need. The larger value tells you the number of shards
1. Data to be ingested
2. Data to be consumed

Main Factors
1. Average size of data records in KB
2. Number of records per second
3. Number of consumers

Calculate amount of data to be ingested
	# of records X average data record size in KB

Calculate amount of data to be consumed
	incoming write/ingestion bandwidth in KB X # of consumers
