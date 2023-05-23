# Amazon CloudWatch

**Global service** designed to help monitor health and performance of your resources
- Heavily used by operational roles and site reliability engineers

**Components**
- CloudWatch Dashboards
	- Can build and customize dashboards using visual widgets
	- Can be done via console, CLI, or API
	- Can include resources from multiple regions in the same dashboard
- ClouWatch Metrics and Anomaly Detection
	- Monitor a specific **metric** of an app or resource over a period of time
	- By default, free set of metrics are collected over a period of 5 minutes
	- Can enable **detailed monitoring** for a metric, which will collect data every minute
	- Can create **custom metrics** for your apps
	- Custom metrics are regional
	- **Anomaly detection** - employs machine learning against your metrics to detect activity that is unusual
- CloudWatch Alarms
	- Tightly integrates with metrics
	- Can automatically take action when thresholds for a metric are exceeded
	- 3 Alarm States
		- `OK` - metric is within threshold
		- `ALARM` - metric has exceeded threshold
		- `INSUFFICIENT_DATA` - alarm just started, metric is not available, or not enough data has been collected yet
	- Example: an alarm when EC2 instances scale in/out (horizontal scaling)
- CloudWatch EventBridge
	- [See below](#cloudtrail-eventbridge)
- CloudWatch Logs
	- Gives you a _centralized location_ to house all of your logs from different AWS services (i.e. CloudTrail, EC2, Lambda logs) and your own applications
- CloudWatch Insights
	- Provides ability to get more information from the data that CloudWatch collects
	- 3 types of insights
	- **Log Insights** - analyze CloudWatch logs at scale in seconds, using interactive queries and visualizations
	- **Container Insights** - aggregates and group different metrics from different container services and container applications within AWS
		- Can be analyzed at the cluster, node, pod, and task levels
	- **Lambda Insights** - aggregates system and diagnostic metrics related to Lambda
		- This must be enabled on a **per-lambda basis**

**Unified CloudWatch Agent** will collect logs and other metrics from EC2 instances and on-premise services running either Linux or Windows

## CloudWatch EventBridge

- An option for implementing event-driven architecture
- A way to connect your apps to **targets**, which allows you to implement real-time monitoring and respond to **events** that occur in your application as they happen
- An **event** is anything that causes a change to your environment or application

## Components of EventBridge

**Rules** - filters incoming events, and routes them to the appropriate target (as defined in the role)
- A single rule can route traffic to multiple targets

**Targets** -where events are sent by rules
- Examples: Lambda, SQS, Kinesis Stream, SNS

**Event Bus** - component that receives the event from your applications
- Rules are associated with a specific event bus
- CloudWatch EventBridge is the **default** event bus
