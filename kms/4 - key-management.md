# Key Management

Topics
* Rotation of CMKs
* Import key material from an existing KMS outside of AWS
* Deletion of CMKs

## Rotation of CMKs

The longer a key is let in place, and the more data that is encrypted with that key, then if that key is breached then there's a much wider blast area

### Automatic Key Rotation

KMS automatically rotates your keys every 365 days (1 year) **if your key was not made from imported key material**
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
* Because you can import key material, you can technically create a CMK without any _initial_ key material
* When using external key material, it becomes tied to the CMK and no other key material can be used for that CMK

When importing material, KMS will provide you a "wrapping key" or public key, and an import token. These provide you a secure way of uploading your own key.
* Use the wrapping/public key to encrypt your private key.
* The import token ensures that the upload process was correct and complete.
* The wrapping key and import token expire after 24 hours.

Can optionally set an expiration of the imported key material. When it expires, the key material is deleted.

## Things to Consider

* Key material created by KMS has higher durability and availability
* For key material provided by KMS, you cannot set an expiration time (but you can for imported keys)
* In a region-wide failure, you will need to re-import the key material back into your CMK.

## Deleting a CMK

KMS enforces a scheduled deletion process, ranging from 7-30 days.

When CMK is pending deletion, it cannot be used for encryption/decryption, nor can it be rotated.

For CMKs with imported key material, you can delete just the key material and not the CMK.

If you don't want to delete the CMK, you can instead disable it.

**Recommended** Setup a Cloudwatch alarm when someone tries to use a CMK for encryption/decryption
* Useful when your key is pending deletion, because these alarms can notify you that the key is still being used and that you should act accordingly.
