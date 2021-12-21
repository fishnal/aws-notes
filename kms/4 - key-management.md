# Key Management

Topics
* Rotation of CMKs
* Import key material from an existing KMS outside of AWS
* Deletion of CMKs

## Rotation of CMKs

KMS rotates your keys every 365 days (1 year)
* This cannot be changed
* However, can still do manual key rotations (but this is a bit of a headache, see below...)
* If a CMK is disabled or pending deletion, then KMS will not rotate it until it is re-enabled or deletion is cancelled.
* Cannot manage key rotation for any AWS managed CMKs; these rotate every 1095 days (3 years)

KMS does not delete the CMK, instead it will basically change/modify the existing CMK by rotating it's underlying key.

KMS will also retain the previous/old underlying key in order to decrypt ciphertexts that were encrypted using the old key.
* If a breach has already occurred, then rotating the key will not stop hackers from decrypting *old* data
* However, key rotation will stop hackers from decrypting *new* data

### Manual Key Rotation

1. **Do not delete the old CMK**. These can still be used to decrypt any data that was encrypted via said CMK.
	* Better rule: do not delete CMKs that were used to encrypt data.
2. Create a new CMK
3. Need to update any applications to reference the new CMK-ID
4. Can use aliases for your keys, and then update the alias target to point to the new CMK-ID (this is way more manageable than updating every application...)

## Importing Key Material

**Key material** is basically the underlying key.

When creating CMKs, you can choose either to use (1) KMS key material or (2) external key material, which means importing your own key.
* Your key must be in a binary format

KMS will provide you a "wrapping key" or public key, and an import token. These provide you a secure way of uploading your own key.
* Use the wrapping/public key to encrypt your private key.
* The import token ensures that the upload process was correct and complete.
* The wrapping key and import token expire after 24 hours.

Can optionally set an expiration of the imported key material. When it expires, the key material is deleted.

## Things to Consider

* Key material created by KMS has higher durability and availability
* In a region-wide failure, you will need to re-import the key material back into your CMK.

## Deleting a CMK

KMS enforces a scheduled deletion process, ranging from 3-30 days.

When CMK is pending deletion, it cannot be used for encryption/decryption, nor can it be rotated.

For CMKs with imported key material, you can delete just the key material and not the CMK.

If you don't want to delete the CMK, you can instead disable it.
