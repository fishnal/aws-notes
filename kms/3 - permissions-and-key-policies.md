# Permissions and Key Policies

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
In order to do this, the root user must have full access to the CMK:
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
* Can only be created using the KMS API
* Even though a Grant may be created, it may take some time to take effect (b/c of eventual consistency)
