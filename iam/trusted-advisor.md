# Trusted Advisor

Helps optimize your infrastructure by recommending best practices and improvements.

Uses a **service-linked role called `AWSTrustedAdvisorServiceRolePolicy`** to access your resources

## 5 Categories
- Cost optimization (i.e. prefer reserved capacity or remove unused capacity)
- Performance (i.e. provisioned throughput)
- Security
- Faul Tolerance - increasing resiliency and avaialability
- Service Limit - when resources reach 80% capacity of their service limit quota

## Different Support Plans
Not all advisor checks are available in certain support plans
- Basic and Developer gives **6 core security checks** and service limits
	- S3 bucket permissions
	- Security groups - specific ports unrestricted
	- EBS public snapshots
	- RDS public snapshots
	- IAM use
	- MFA on root account
- Business and Enterprise have the most # of checks
	- Track most recent changes to your AWS account
	- Use AWS Support API to retrieve and refresh advisor results/scans
	- CloudWatch integration to detect and react to changes made to your advisor checks

## Features available to everyone
- Trsuted Advisor Notifications - optional feature that tracks your resource check changes and cost saving estimates over a week
	- Can email this report to a configured billing, operations, and security contact
- Exclude Items - exclude certain resources from appearing in the console for advisor checks
- Action Links - hyperlinks that are specified in advisor checks, gives you much quicker access to the resource
- Access Management - can grant different levels of access to advisor
	- i.e. Full access, read only, or restrict access to specific categories, checks, and actions
- Refresh
	- By default, data within Trusted Advisor is refreshed **when** you visit the console **and** the data is older than 24h
	- Can perform manual refresh 5 minutes after the previous refresh
	- Can refresh individual checks or all checks
