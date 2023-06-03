# Permissions and Key Policies

**Key policies** are _resource-based policies_ create within KMS and are tied to a KMS key
- A key can have only 0 or 1 key policies
- Specifies the following
	- If IAM user permissions are allowed to control access to the key
	- Who can administer the key
	- Who can use the key for cryptographic operations
	- Who can create **grants** for temporary access

**Must** use key policies to manage access to CMKs

Can also use key policies with IAM and grants.
* Can gain access to a CMK through key policies, IAM policies, and grants.

## Using Key Policies

Every CMK must have a key policy.
* KMS creates a default key policy
* Root user of AWS account has full access to the CMK
* Root user can use IAM to manage access for other users and groups
* If a user had full access to a CMK and the user was later deleted, then contact AWS support to regain control of the CMK

### When creating a CMK...

Each CMK has a set of key administrators, who can only administer permissions for that CMK.
* Key admins cannot use/encrypt with the CMK, however they can grant themselves permission to do so by updating the key policy
* Optional checkbox for whether key admins can delete the CMK

CMK has a set of users who can use the key for encryption.
* These users can also use **grants** (see below) to give a subset of their permissions to another principal (i.e. AWS service integrated with KMS, another user)

Key policies can restrict access via `"Effect": "Deny"`

## Using Key Policies with IAM Policies

In order to "respect"/allow IAM policies, you must have a key policy statement that allows this. Without the below statement, IAM permissions are ignored (unless they are explicit DENY)

```json
{
	"Sid": "Enable IAM User Permissions",
	"Effect": "Allow",
	"Principal": {"AWS": "arn:aws:iam:123456789123:root"},
	"Action": "kms:*",
	"Resource": "*"
}
```

## Using Key Policies with Grants

Grants allow a user to give a subset of their permissions to another AWS principal
* Can only be created using the KMS API; cannot create via console
* Even though a Grant may be created, it may take some time to take effect (b/c of eventual consistency)
	* If you need to get around this, you can use a **GrantToken** (issued after creating the grant) with some APIs
* A grant is only available with a single KMS key
* Grantee principal must be a user, role, or federated user/role.
* Grantee principal CANNOT be IAM group, AWS organization, or AWS _service role_
* Statements for grant operations only apply to `Allow` operations; you cannot add a `Deny` action in a grant

## Policy Evaluation Logic
