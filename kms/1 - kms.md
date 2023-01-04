# Key Management Service (KMS)

Service used to store and generate encryption keys

KMS also contains keys used to decrypt your data

KMS is only for encryption at rest.
* KMS **will not** encrypt data in transit. If you want data to be encrypted in transit, consider using SSL.

KMS works with AWS CloudTrail, so you can track how keys are being used and by whom.

KMS is **region specific**, however multi-region keys are now supported for client-side encryption
