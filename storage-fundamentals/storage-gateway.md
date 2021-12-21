# Storage Gateway

The storage gateway is a software appliance that integrates on-permise storage with AWS storage.
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

**Stored volume gateways** are typically used for backing up data to S3, while also ensuring your data is still stored locally.
* Accessing and modifying data is done through your local storage services
* Whenever data is written, it is first written to your local storage services. And then *asynchronously*, the volume gateway will copy this data to S3 as EBS snapshots.
* All data is server-side encrypted
* Volumes can be between 1GB-16TB, and each gateway can hold 32 volumes

**Cached volume gateways** differ from stored volume gateways in that the S3 bucket is the primary data storage. Instead, your local storage services are used as buffering and local cache
* All data is server-side encrypted
* Each volume can be up to 32TB, and each gateway can hold 32 volumes

**Tape gateway**, or a **Gateway-Virtual Tape Library** allows you to backup data to S3, but also allows you to leverage Amazon Glacier for archiving
* Can store 1500 **tapes** in a gateway
* Each tape has capacity ranging from 100GB to 2.5TB
* Each VTL has 10 **tape drives**
* Has a **media changer** that manages tapes between your tape drive and VTL
* Has an **archive** which lets you transfer tapes from the VTL to Amazon Glacier
