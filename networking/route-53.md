# Route 53

**Also take a look at [Route 53 in `dns-and-cdn.md`](/networking/dns-and-cdn.md#route-53)**

DNS provided by AWS

When you use Route 53 to register a domain, Route 53 becomes the **authoritative DNS server** for that domain, and creates a **public hosted zoned**
- **Public hosted zones** define how traffic is routed on the public internet
- **Private hosted zones** define how traffic is routed inside a VPC
	- VPCs that use private hosted zones **must** enable `DNS Hostname` and `DNS Support`

Private and public zones are made up of **records**

## Record Types

| Abbreviation       | Name                  | Description                                                                                            |
| ------------------ | --------------------- | ------------------------------------------------------------------------------------------------------ |
| NS                 | Name Server           | Identifies DNS servers for a given hosted zone                                                         |
| SOA                | Start of Authority    | Defines authoritative DNS servers for an individual DNS zone                                           |
| A                  |                       | Used for IPv4 addresses                                                                                |
| AAAA               |                       | Used for IPv6 addresses                                                                                |
| MX                 | Mail Exchange         | Identtifies email servers for a given domain                                                           |
| TXT                | Text                  | Provide information in text format to systems outside of your domain                                   |
| CNAME              | Canonical/Common Name | Maps one hostname to another hostname. Used for mapping multiple names to the same host                |
| **AWS ONLY** ALIAS |                       | Maps a hostname to an AWS resource (usually an _internal_ domain name that AWS made for that resource) |

Route 53 wqill create 4 unique NS records and 1 SOA record in each hosted zone.

Records have a TTL, which is how long a record is valid, after which the DNS should be queried again for this record.

**Routing policies** for a record define how to answer a DNS query
- See [health checks](#health-checks) and [routing policies](./dns-and-cdn.md#routing-policies)

## Health Checks

Records can add health-checks to their routing policies.
- DNS queries for this record will then perform a health-check to the record's endpoint every 30 seconds (this interval can be changed)
- Based on the health responses, Route 53 will respond to the DNS query accordingly.
- For HTTP protocols, you can (optionally) search for a matching string in the health-check response body. **The string must appear in the first 5120 bytes of the response body.**

Can setup CloudWatch Alarms with these health-checks

If a record does not have a health-check, then Route 53 assumes the record is always healthy.

## Traffic Flow

Simplifies process of creating and maintaining records in large and complex configurations

Useful when you have a group of resources that perform the same operation (i.e. fleet of web servers for the same domain)

Visual Editor
- Create complex sets of records
- Visualize relationships between records and record sets
- Combine multiple routing policies and health checks into a single config
- A configuration is called a **traffic policy** and is automatically versioned
  - Older versions are available until you delete them
  - Can define hundreds of records, which are then automatically created by Traffic Flow when you create a **policy record**
- **Policy records** associate traffic policies with a domain or subdomain name records
	- Appears in the list of records for the hosted zone
- Can use the same traffic policy to create records in multiple public-hosted zones (useful when you're using the same hosts for multiple domains)
- Geoproximity routing policy is only available when you use Traffic Flow

## Resolver

DNS service for VPCs that integrates with your data center

Connectivity must be established between your data center DNS and AWS using a Direct Connect (DX) or VPN

Configure endpoints for DNS queries into and out of VPCs
- Endpoints are configured through an IP address assignment in each subnet that needs the Route 53 Resolver

**Inbound queries** allow DNS queries that originate in your data center to resolve AWS-hosted domain names

**Outbound DNS queries** are enabled using conditional forwarding rules
- Domains hosted in your data center can be configured as forwarded rules in Resolver
- Rules trigger when a query is made to that domain

## Resolver DNS Firewall

Managed firewall service for DNS queries that start in your VPCs

Use a **firewall rule group** to inspect and filter traffic coming from your VPC

Rules contain a domain list to inspect in DNS queries, and an action to take when a domain is matched
- Can block a DNS request with a default or custom response

To apply the DNS firewall, associate the rule group with your VPCs that you want to protect

## Application Recovery Controller

Continuously monitors an app's ability to recover from failures and manages app recovery across multiple AZs, regions, and your own data centers

Allows you to configure fine-grain failover and verification steps to implement apps that require very high availability

Define a **readiness check** to monitor AWS resource configs, capacity, and network routing policies.
- Can check the configuration of auto scaling groups, EC2 instances, EBS volumes, ELBs, RDS instances, DDB tables, and more
- Ensures recovery environment is scaled and configured to take over when needed
  - Check AWS service limits to verify enough capacity can be deployed
- Works with **routing controls** in order to failover an entire app based on custom conditions (i.e. app metrics, partial failures, latencies).
  - Can also failover manually

**Routing Controls** can shift traffic for maintenance purposes or a real-life scenarios
- Essentiallyh turns a traffic flow ON/OFF to individual cells in regions or AZs
- Can apply **safety rules** to routing controls as a way to prevent a failover to an _unprepared replica_

**Control panel** is a group of routing controls for an app
- Lets you define custom traffic re-direction for your apps during failover or maintenance
