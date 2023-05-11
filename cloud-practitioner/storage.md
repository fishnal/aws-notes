# S3

## S3 Overview
[See S3](/storage/s3.md)

## S3 Storage Classes

See the following:
1. [Storage Classes](/storage/s3.md#storage-classes)
2. [S3 Glacier](/storage/s3.md#amazon-s3-glacier)

# EC2 Instance Storage

[See EC2 Instance Storage](/storage/ec2-instance-storage.md)

# Elastic Block Store (EBS)

[See EBS](/storage/ebs.md)

# Elastic File System (EFS)

[See EFS](/storage/efs.md)

# Snow Family

Different Snow devices
1. Snowcone
	- Comes with 8TB and an EC2 instance
	- 10 Gbit transfer speed
	- Lightweight
	- Very portable (can fit on a drone)
	- Can use almost anywhere
	- Rugged casing
	- Can be battery powered
	- Can perform online transfers via AWS DataSync
2. Snowball
	- Up to 80TB
	- 100 Gbit transfer speed
	- Not very portable since it's larger than snowcone
	- More compute power
	- Rack mountable
	- Contains SSD storage
	- HIPAA compliant
	- Can be **clustered** in groups of 5-10 devices (see below)
	- No battery expansions
	- **Storage Optimized Snowball** is for data transfer and is compatible with EBS volumes and S3
	- **Compute Optimized Snowball** is for computing workloads in a disconnected environment
	- **Compute Optimized with GPU** is for accelerated AI, HPC, and graphics
3. Snowmobile
	- Up to 100PB
	- Typically used when changing physical data centers
	- Typically used when you need to transfer more than 10PB
		- For anything less, use multiple snowballs
	- Can use multiple snowmobiles in parallel, allowing you to transfer EB (exabyte) of data
	- Comes with connector rack, so you can connect it to your own data center (up to 2km of network cabling)

**Clustering snowball devices** allows you to combine multiple snowballs to act a single pool of resources.
- Larger storage capacity
- Increases durability of data should a snowball fail
- Only an option if you want to perform local compute and storage workloads, without needing to transfer data to AWS

## Key Features of Snow Family

Common features in ALL snow devices
- Encryption via KMS
	- For security purposes, encryption keys **are not** stored on the devices during transit
- Uses E-Ink shipping label, which can be tracked via AWS SNS, text messages, and AWS **console**
- Once device is no longer needed, AWS performs a _secure erase_ that follows the NIST standard

Snowcone and Snowball Common features
- Anti-tampering casing
- Verification checks when first switched on
- Physically durable
	- Can operate between -32C to 64C
	- Windproof
	- Dustproof
	- Water-resistant
	- Vibration dampening
	- Shockproof

Snowmobile features
- Can only be operated by AWS personnel
- Can be escorted by additional security vehicles
- GPS tracking
- 24/7 video surveillance and alarms
- Can operate up to 29.4C (if temps exceed this, then additional chilling units can be provided)
- If your data center does not have enough power for the snowmobile, then AWS can send a separate generator. However, this generator takes up the same space as a snowmobile

## OpsHub

- A GUI to manage your snow devices
- **Not an AWS managed service**, this is instead just an application that you download
- Can perform simple drag/drop operations (in addition to using AWS CLI or AWS APIs)
- Can configure a cluster of snow devices
- Displays metrics related to storage and compute resources
- Integrates with AWS Systems Manager
