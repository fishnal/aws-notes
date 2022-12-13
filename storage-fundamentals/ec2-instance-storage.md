# EC2 Instance Store Volumes

- Directly attached storage
* Instance store volumes provide ephemeral (temporary) storage
* If instance is stopped or teminated, then data is lost
* If instance is rebooted, the data is in-tact
* No additional cost for storage, it's included in the price of your EC2 instance
* I3 Instance family
  - Fast I/O speeds
  - 3.3 million random read IOPS
  - 1.4 million write OPS
- Can be good for caching
- Can be used within a load balancing group, where data is replicated and pooled between the group
* Can be accessed by multiple EC2 instances
- **Note:** instance store volumes are not available for all EC2 instances
- Capacity of volumes increases with size of EC2 instance
- Volumes have same security mechanisms provided by EC2

Do not use instance store volumes for:
- Data that needs to be persisted
- Data that needs to be shared between multiple entities
