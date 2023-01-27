# Simple Notification Service (SNS)

Publish/Susbcribe Model
- Messages are published in topics, which is a group of messages
- When messages are published, ALL subscribers in that topic are notified

A service can subscribe via
- HTTP/HTTPS
- Email
- Email-JSON
- SQS
- Applications
- Lambda
- SMS

Topics can restrict what types of subscribers are allowed (i.e. a topic may only allow SMS subscribers)

Policies for topics follow same format as IAM policies
