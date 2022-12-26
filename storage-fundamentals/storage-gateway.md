# Storage Gateway

The storage gateway is a software appliance that integrates on-permise storage with AWS storage.
* Software can be downloaded as a VM
* In general, data will be stored in an S3 bucket
* All data is server-side encrypted with S3-managed keys (SSE-S3)

## Gateway Configurations
* File gateway
* Volume gateway
* Tape gateway

**File gateways** securely store your files in an S3 bucket, and allow you to mount/map drives to an S3 bucket
* In simpler terms, it would allow you to access an S3 bucket as if it were a physical drive connected to your computer.
* All network traffic is done over HTTPS
* All data is server-side encrypted
* Provides a local cache to reduce latency and network traffic to the S3 bucket

**Stored volume gateways** are typically used for backing up data to S3, while also ensuring your data is still stored on-premise.
* Accessing and modifying data is done through your local storage services
* Volumes are mounted as iSCSI devices which are then mapped to your on-premise storage solutions (like NAS/SAN/DAS)
* Whenever data is written, it is first written to your local storage services. And then *asynchronously*, the volume gateway will copy this data to S3 as EBS snapshots.
* All data is server-side encrypted
* Volumes can be between 1GB-16TB, and each gateway can hold 32 volumes
* Max storage of 512TB per gateway
* Storage buffer using on-premise storage is used as a staging point for data
* Data is uploaded through SSL channel and then encrypted at rest
* Snapshots can be taken of volumes at any time, stored as EBS snapshots on S3

**Cached volume gateways** differ from stored volume gateways in that the S3 bucket is the primary data storage. Instead, your local storage services are used as buffering and local cache
* Volumes are mounted as iSCSI device
* All data is server-side encrypted
* Each volume can be up to 32TB, and each gateway can hold 32 volumes
* Max storage of 1024TB per gateway
* Snapshots can be taken of volumes at any time, stored as EBS snapshots on S3

**Tape gateway**, or a **Gateway-Virtual Tape Library** allows you to backup data to S3, but also allows you to leverage Amazon Glacier for archiving
* Can store 1500 **tapes** in a gateway, each ranging in capacity from 100GB to 2.5TB
* Each VTL has 10 **tape drives**
* Has a **media changer** that manages tapes between your tape drive and VTL
* Has an **archive** which lets you transfer tapes from the VTL to Amazon Glacier
* Your apps and backup software can mount tape drives by using the media changer as an iSCSI device

## Pricing

Determined by storage, # of requests, and amount of data transfered
