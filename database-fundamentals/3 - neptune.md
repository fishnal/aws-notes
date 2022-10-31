# Neptune

Fully managed graph database service

A bit more niche than the other database services, but is still really useful

Best suited for lots of data that is interconnected with a variety of complex relationships. In these cases, relational and non-relational databases are actually more difficult to use with this kind of data.

Supported Query Languages
- Apache Tinkerpop Gremlin
- W3C SparQL

# Definitions

Neptune DB Cluster
- Contains 1+ DB instances across different AZs
- Contains a virtual DB cluster volume, which contains data across all of the instances
	- The cluster uses multiple SSDs
	- This shared volume will auto-scale up to a max of 64TB
- Each cluster stores keeps it's own copy of the shared cluster data
	- Good for durability and maintainability
	- **Neptune Storage Auto-Repair:** for a given cluster, will look at segment failures in an SSD and repair that segment based on data from other clusters
- Support for read-replicas (max 15 replicas)
	- If the primary instance fails, then a replica in another AZ (different from primary instance AZ) will be promoted to primary (takes about 30s)

# Connecting with Different Endpoint Types
- Cluster
	- Points directly to primary instance
	- Used for read/write access
- Reader
	- Used for read-only access
	- There will only be 1 reader endpoint, even if you have multiple replicas
	- Replicas are used on a round-robin basis
	- Does not load-balance your traffic
- Instance
	- Each instance within a cluster has a unique endpoint
	- Allows you to route traffic to specific instances within the cluster (typically for load balancing)

# Use cases
- Social networking: good for optimizing a feed to display content based on relationships of the current user (i.e. places they've liked, friends, mutual friends, activities they like, etc)
- Fraud detection: for analyzing financial relationships to help detect fraudulent activity
- Recommendation engines: similar rationale as social networking
