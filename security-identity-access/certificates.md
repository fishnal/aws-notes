# Certificates

## Certificate Authorities (CAs)

CAs are used to validate and establish trust for an SSL certificate
- Generally, computers will have a set of trusted CAs, so certificates signed by those CAs are also trusted

Public CA
- Trusted by most devices
- Issues certificates to businesses with embedded public keys that can be trusted
- Issues certificates that are (usually) used on public-facing services
- Costs money to have a Public CA sign your certificate

Private CA
- Same functionality as a Public CA, except they are not trusted by most (or all) devices out of the box
- Must import a private CA's **root certificate** into your **root certificate store**
- Does not cost any money since they are for private use
- Usually for internal use

## AWS Certificate Manager (ACM)

ACM is a **regional service**
- **Exception:** If you want to create a certificate for a CloudFront distrobution, then you must create the certificate from `us-east-1`...

Benefits
- Publicly trusted certificates are available for free
- Key pairs and CSRs are created automatically when you request a new certificate from ACM
- Automatically rotates expiring keys

### Creating a Public Certificate via ACM

- Must specify a domain name
- Must validate domain ownership
	- DNS validation - must add a CNAME record for each domain listed in your request
		- If using Route 53, ACM does this for you
		- Otherwise, add the record yourself
	- Email validation - email is sent to the owners of the domain (which is fetched from a WHOIS lookup)

### Using an Private CA in ACM

To use an ACM Private CA, you must
- Create a certificate hierarchy
- Configure a root CA
- Configure a subordinate CA

Chain of Trust
1. Starts at the root CA.
2. Root CA signs certificates (via root CA's private key) from subordinate CAs
3. Subordinate CAs sign their certificates with their private key
4. Any certificate that is signed by the root CA in the chain can be trusted

Use Cases
- Internal services hosted in AWS or on-premise
- Certificates needed for internal domains
- Simplifies CA management by shifting responsibility to AWS

Pricing
- Monthly fees for each private CA created
- One-time fees for each certifiate signed by private CA
