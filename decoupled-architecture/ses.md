# Simple Email Service (SES)

Automated email system to communicate with your customers

- Great for marketers and developers
- Used to send confirmation emails
- Can design your own applications to automatically respond to/handle incoming emails with SES
- Easy to integrate into SMTP interfaces

You can add an email address to SES (the email will receive a confirmation link)
- With this email address, you can send emails from *this same address* via SES (either in the console, or programatically)
- When receiving emails, AWS will auto scan for spam, viruses, and reject messages from untrusted sources

## Recipient-Based Control
- Set up to direct emails based on conditions
- Receipt rules are used to determine how the email is handled
	- S3: email delivered to a bucket
	- SNS: email is published to SNS topic
	- Lambda: a lambda is invoked with an SES event
	- Bounce: rejects the email and returns a "bounce" response to the sender
	- Stop: stops evaluating the receipt rules (**does this mean the email is dropped?**)
	- Add Header: adds a header to the email
	- WorkMail: uses Amazon WorkMail to process the email
- If no matching recipient is found, the email is discarded

## IP Address-Based Control
- Allows/blocks emails based on IP address
- Maintains and updates its own list of known spamming sources
- By default, all emails that originate from EC2 instances are **blocked**
- **Note:** IP address based control is applied BEFORE receipt rules
