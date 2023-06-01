# Firewall Manager

Helps simplify management of providing security to a range of different resources, across multiple AWS accounts
- A lot of power comes from managing the "multiple account infrastructure"

**Requires AWS Organizations to be enabled**

Simplifies a lot of administration:
- Configure security policies that you need
	- This can be done per account as well
- Firewall Manager will then auto apply these policies for any newly created resources

Integration with other AWS services
- WAF
- Shield Advanced
- Network Firewall
- VPC Security Groups
- Route 53 Resolver DNS Firewall

## Using Firewall Manager
1. Select an account for the Firewall Manager Administrator account
2. Enable **AWS Config** for the following
	1. Firewall Manager admin
	2. Any account in the org that you want to manage resource security for
	3. In every region for each account
3. If you want to apply security policies for all Network Firewalls and DNS Firewalls, then enable **sharing with AWS Organizations** in AWS Resource AccessManager
4. Enable regions to allow Firewall Manager to manage resources in that region
