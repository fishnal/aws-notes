# S3 Lifecycle Configurations

The **S3 Lifecycle** refers to moving objects into different S3 storage classes (or even _deleting_ objects) in order to data retrievals and optimize costs
- Great for objects and workloads with predictable patterns of usage
- Examples
	- You are required to retain logs for up to 3 years; after 3 years they can be deleted
	- You are required to keep certain data for an unspecified time; so you can use lifecycles to move the objects into S3 Glacier after a couple months
	- Can use S3 Intelligent-Tiering

**Do not confuse S3 Lifecycles with S3 Intelligent-Tiering**

When an object is moved to a lower-cost storage class, it cannot move "back up". It can only move down. Here is a list of storage classes:
1. Standard/Reduced Redundancy
2. Standard IA
3. Intelligent-Tiering
4. One Zone IA
5. Glacier
6. Glacier Deep Archive

Example
- Suppose you have two rules in your lifecycle
- Rule #1 moves objects to Glacier
- Rule #2 moves objects to Intelligent-Tiering
- Suppose an object is acted on by Rule #1; so the object is now in Glacier
- Next, that same object is acted on by Rule #2. However, since objects can only "go down" in storge classes, and since the object is in Glacier, this rule cannot move the object into Intelligent-Tiering

## Lifecycle Configuration Structure

A lifecycle configuration is defined in a XML file
- However, you can also define it in JSON if you interact with S3 lifecycles via CLI

A configuration is made up of rules

Rules contain the following fields
- `ID`
- `Filter` - which objects to take action on
- `Status` - whether the rule is enabled or disabled (useful for testing or troubleshooting)
- One or more actions
	- `Transition` - move objects to another storage class
	- `Expiration` - deleting objects
	- `NoncurrentVersionTransition`
	- `NoncurrentVersionExpiration`
	- `ExpiredObjectDeleteMarker` - used for objects that have 0 versions but still have a delete marker left; this action will remove the object
	- `AbortIncompleteMultipartUpload` - used when you need to clean up incomplete multipart uploads; can specify max number of days the multipart uploads remain "in progress"

### Lifecycles with Object Versioning

If object versioning is enabled, then lifecycle actions are performed for the current version of your object

If you want to transition or delete a non-current versions of an object, use the `NoncurrentVersionTransition` or `NoncurrentVersionExpiration` action respectively
- Both actions can be configured based on
	1. Number of days since object has been noncurrent
	2. Maximum number of versions to retain

## Pricing and Considerations

- Cost for each object acted on
- Transitions to a storage class costs money
- Transition costs increase as you transition to a lower-cost class
- **Minimum storage durations** increase as you go to lower-cost classes (this is not a financial cost, but a time cost)
	- A **minimum storage duration** is how long before you can delete, update, or transition an object
	- If you delete/update/transition an object during this period, you still pay for the minimum storage duration
	- Example minimum duration is 30 days, you delete object 3 days after uploading, but you still pay for hosting the object for 30 days
