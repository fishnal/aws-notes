# Amazon S3 Glacier

* Made for long-term, durable storage; also known as **cold storage**
* Storage costs are much cheaper
* Retrieving data can take significantly longer

## Vaults & Archives

An **archive** is like an object in S3: it's just data (music, videos, text, etc).

Archives are then stored in a **vault**.
* Vault can hold unlimited archives
* Vaults are regional

**Note:** Glacier makes use of archives and vaults, not buckets and folders

## Glacier Dashboard

Only allows you to create vaults. You **CANNOT** upload or download any data from the dashboard.

Uploading and downloading archives can only be using code
* Glacier REST API
* AWS SDKs

## Moving Data in Glacier
1. Create a vault
2. Upload data into the vault via the API/SDK or an S3 Lifecycle rule

## Data Retrieval
* Standard
* Expedited (most expensive)
* Bulk (cheapest)

You must submit a retrieval request in order to access your data in Glacier.
* When ready, Glacier will store your data in a temporary object in an S3-RRS or S3-IA storage class bucket.
* The original archive is left untouched and remains in the Glacier storage class.

**Standard** gives you any amount of data within 3-5 hours
- 2nd most expensive retrieval option

**Expedited** gives you data within 1-5 minutes, but the data must be less than 250MB
- Most expensive retrieval option

**Bulk** gives you petabytes of data within 5-12 hours, but is the cheapest option.
- Cheapest retrieval option

## Comparing Glacier Storage Classes
![Comparing storage classes](./assets/s3-overview-storage-classes.png)

## Security
* Data is encrypted by default (via AES-256)
* Vault Access Policies
* Vault Lock Policies

**Vault access policies** are resource-based policies
* Each vault can have only **1** vault access policy
* Policy contains a "principal" component to identify who is granted/denied access

**Vault lock policies** are vault access policies, except once the policy is set, it **CANNOT** be changed
* These are typically used to maintain governance and compliance
* ex) Any data stored in the vault cannot be deleted if they are less than 100 days old.
