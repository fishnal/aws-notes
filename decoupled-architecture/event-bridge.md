# EventBridge

EventBridge is a serverless **event bus**
- An **event bus** is like an event-coordinator
- Can take in information from external SaaS providers, other AWS services, or custom apps
- EventBridge will filter and direct events to other systems and take action based on certain events

Events are JSON strings

90+ services have events

EventBridge is the only event-based service that integrates directly with 3rd party software as a service provider
- To do this, you have to associate events with a **partner event bus**

## Rules

In an event bus, **rules** determine which **target** to forward the event to
- For example, an event for "S3 object created" should be forwarded to a Kinesis stream

Rules are not processed in any particular order

Can have up to 100 rules in a single event bus

Rules can send events to another AWS account, called **cross-account events**

## Targets

Only 15 services are supported as targets

## EventBridge Archive

Can archive events and replay them at a later time
- Events can be archived indefinitely, but you can also set a finite retention period

Use Cases
- Disaster recovery: replay events prior to the disaster to help re-build your system
- Updating an app with a new feature

Can only replay events on their original event bus

## Schema Registry

Great for specifying which fields are required/optional in an event

EventBridge has a schema registry built-in
- Each AWS service available for EventBridge has a prebuild schema in the registry

Can discovery schemas based on events that flow through the bus

Can create custom event schemas

## Contrast with SNS

Prefer SNS for very simple pub/sub. Otherwise prefer EventBridge for more complex scenarios.

SNS allows you to scale up to millions of subscribers, but does not have direct connectivity to software as a service provider.
- More difficult to trigger step functions with SNS than with EventBridge
- Has great scaling, but filtering is limited to only attributes; filtering does not work on the _event's content_

SNS doesn't provide as much routing capability as Amazon event bridge does.

## Contrast with Kinesis

Kinesis works great as event storage (and it's also real-time), but it's limited to the number of consumers connected to the stream

Each Kinesis consumer would also have to do filtering themselves
