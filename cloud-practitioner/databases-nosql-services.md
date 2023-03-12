# Managed NoSQL services on AWS

## Key-Value DBs
**AWS service is DynamoDB**
- Designed for storing, retrieving, and managing **associative arrays**
	- **Associative arrays** are dictionaries or hash tables
- Suited for large amounts of data
- Values can be text, numbers, a list, documents, or another key-value pair
	- Although you can store binary data as a value, it's usually more efficient (time and cost) to save the binary data in an object storage, and then use your database as a lookup for that object
- Queries are very simple, since lookups are based on just the key
- Not optimized for searching: very expensive to scan in terms of time and compute
- Not good for apps that require frequent updates or complex queries
- **Note:** Because DynamoDB supports storing key-value pairs as a value, it is therefore a type of **document database**

Use Cases
- Storing really simple data
- Commonly used for in-memory data caching

## Document DBs
**AWS service is Amazon DocumentDB**
- Stores semi-structured data
- Designed for JSON data
- Amazon DocumentDB will **create metadata** from how data is structured **for optimizing the database and queries**
- Scales horizontally
- Data inside a document can be queried; whereas in a key-value store, a query returns the value in its entirety

Use Cases
- Web apps
- User-generated content
- Shopping catalogs
- Gaming
- Gathering data from IoT devices

## Column Store
**AWS service is Amazon Keyspaces**
- Stores data using column-oriented model
- Columns allow database to precisely access data needed for a query, without having to scan every row
- Uses **keyspaces** to define the data it contains
	- Contains a collection of **column families** that look like tables
- **Column families** contain rows, and rows contains columns
	- Every row can contain a different number of columns
	- Every column is limited to its row, and they do not span all rows (like a relational DB does)
	- A row has a **unique row key**, and a column contains a name, value, and timestamp (when the data was upserted). Timestamp is used to determine most recent version of data
- Some column stores support **composite columns**, which allows objects to be nested inside a column

Benefits of Column Store
- Efficient at data compression and partitioning
- Efficient at aggregation-type queries
- Can calculate SUM, COUNT, and AVG easily
- Scale well, suitable for workloads that do massively parallel processing
- Can be loaded fast and efficiently (1 billion rows can be loaded in seconds)

Use Cases
- Analytical applications
- Data warehousing
- Big data processing

## In Memory Data Stores
**AWS service is Amazon ElastiCache**
- ElastiCache has two NoSQL In-Memory database engines:
	- Redis
	- Memcached
- Used by apps that require real-time access to data
- Since data is stored in memory, these stores can provide microsecond latency
- **Note:** A caching system is not a database. Instead, a cache system sits in front of a database to improve throughput
	- Cache systems also remove the need to add a caching layer inside the application itself
- Many data stores end up holding data that does not change for a while. As a result, it's more efficient to cache these unchanged data because...
	- Some queries can be expensive; so caching only incurs this cost once since the data can be fetched multiple times without having to run the same expensive query
	- Underlying storage devices are slower than RAM
- **Downside:** if the underlying host of your in-memory store shuts down, then your cache is lost
	- However, some in-memory stores like Redis have some level of persistence, by taking snapshots and loading those when the host is back up
- **Note:** Cached data is stale data, so it's important to know how well your applications can tolerate stale data
	- Example: A customer might be ok with a disclaimer saying prices may be delayed by 5 minutes. However, a stockbrocker will want real-time data
- You should cache relatively static data (i.e. a social media profile), not dynamic data
- Suitable as a front-end for relational databases
- Provides high-performance middle-tier for applications that have high request rates or low-latency requirements
	- "Middle-tier" refers to sitting in between the database and application

## Graph Databases
**AWS service is Amazon Neptune**
- Stores data about relationships
- A graph DB is composed of **vertices, edges, and properties**
- Every **vertex** has a unique key-value identifier
- A **vertex** can represent numbers, strings, people, locations, and buildings
- An **edge** is a connection or relationship between two vertices
- Every **edge** is defined by a unique identifier that provides details about the starting or ending node, along with some properties
- Graphs can depict complex relationships between data because both vertices and edges have properties
- Many graph database management systems use their own query language

Use Cases
- Social media networking - process large sets of user profiles and interactions
- Store relationship between customer, friends, and purchase histories
- Fraud detection
- Knowledge graphs
- Modelling life sciences
- Mapping computer networks

## Time-Series Databases
**AWS service is Amazon Timestream**
- Efficiently collect and analyze data that changes over time
- Data is collected at regular intervals
- Time is stored as key, data collected is stored as value
- Computation is usually applied over a range of time data to return a single result
- Primary purpose of a time-series database is to **provide answers**

Use Cases
- Can determine MIN, MAX, and AVG utilization of CPU over a week
- Great for DevOps applications that need to analyze data in real-time to improve performance and stability
- Analyze data generated by IoT sensors, using functions such as smoothing, approximation, and interpolation
- Analyze clickstream data to understand user activity over time
- Analyze data regarding equipment maintenance, trade monitoring, and route planning

## Ledger Databases
**AWS service is Amazon Quantum Ledger Database**
- Provides centralized and trusted authority for a scalable, immutable, and cryptographically verifiable record of transactions
- Trust is maintained by being fully auditable and transparent
- All txns are recorded in a log for tracking purposes
- Since data is immutable, updating data creates a new version of the record
- Changes to database do not overwrite existing records
- Cryptographic Verification - a **hash** is created when a record is committed
	- Uses blockchain when creating hashes: that means a hash is made from the record data and the _hash of the record's previous version_

Use Cases
- Banks often need a ledger to keep track of critical data (such as financial txns)
- Manufacturing companies need to track the manufacturing history of a product (i.e. where each component is assembled)
- Insurance companies need to track history of claims
- HR systems need to track employee details (i.e. payroll, bonuses, benefits, performance history)
- Retailers need to access information on each stage of a product's supply chain

## Search Databases or Search Engines
**AWS service is Amazon ElasticSearch Service**
- Optimized to store and retrieve search-related data
- Offers specialized methods like
	- Full-text search
	- Complex search expressions
	- Ranking of search results
- Take in data from many sources, and then **indexing and enriching** it into **searchable data**
	- An **index** is a collection of documents that are related
	- ElasticSearch stores **indexes** as JSON documents, where each document has key-value pairs
- ElasticSearch also uses an **inverted index**, which provides fast full-text searches
	- An **inverted index** lists every unique word that appears in any document, and associates all of the docs where that word occurs

Use Cases
- Fast, personalized search experience for users
- Real estate businesses can more efficiently filter homes by location, price range, and various other features/amenities
- Analyze network and system logs for real-time threat detection and incident management
-
