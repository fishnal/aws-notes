# Amazon CloudWatch

**Global service** designed to help monitor health and performance of your resources
- Heavily used by operational roles and site reliability engineers

**Components**
- CloudWatch Dashboards
	- Can build and customize dashboards using visual widgets
	- Can run queries within widgets to filter for more detailed data
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
	- Acts as a central repo for _real-time_ logs
	- **Unified CloudWatch Agent** collects _additional_ logs and metrics from EC2 instances, and also from on-premise services running Linux or Windows OS
- CloudWatch Insights
	- Provides ability to get more information from the data that CloudWatch collects
	- 3 types of insights
	- **Log Insights** - analyze CloudWatch logs at scale in seconds, using interactive queries and visualizations
	- **Container Insights** - aggregates and group different metrics from different container services (i.e. EKS and ECS) and container applications within AWS
		- Can be analyzed at the cluster, node, pod, and task levels
	- **Lambda Insights** - aggregates system and diagnostic metrics related to Lambda
		- This must be enabled on a **per-lambda basis**

**Unified CloudWatch Agent** will collect logs and other metrics from EC2 instances and on-premise services running either Linux or Windows

## CloudWatch EventBridge

- An option for implementing event-driven architecture
- A way to connect your apps to **targets**, which allows you to implement real-time monitoring and respond to **events** that occur in your application as they happen
- An **event** is anything that causes a change to your environment or application

### Components of EventBridge

**Rules** - filters incoming events, and routes them to the appropriate target (as defined in the role)
- A single rule can route traffic to multiple targets

**Targets** -where events are sent by rules
- Examples: Lambda, SQS, Kinesis Stream, SNS

**Event Bus** - component that receives the event from your applications
- Rules are associated with a specific event bus
- CloudWatch EventBridge is the **default** event bus

## CloudWatch Dashboards

Common widgets
- Line graph
- Stacked area chart
- Number widget - to see the latest value for a metric (i.e. # of online instances)
- Bar chart
- Pie chart
- Text with markdown support
- Logs table - explore results from CloudWatch Logs Insights
- Alarm status

Can apply math to your data (i.e. data normalization)

Can aggregate metrics from various resources and services

Can create dashboards via JSON code
-

Can add annotations to graphs
- Annotations can be horizontal or vertical

Sharing options
- Share a single dashboard with specific emails and designate passwords
- Share a single dashboard publicly with a link
- Share all dashboards in your account, then specify a 3rd-party SSO provider for dashboard access
  - Must integrate the SSO provider with Amazon Cognito
  - Anyone who is a member of the SSO provider can access the dashboard
  - **This can enable cross-account and cross-region access**

### Best Practices

- Use larger graphs for most important metrics
- Design layout for the average minimum display resolution of your users (i.e. Is everyone using a wide monitor? What about mobile devices?)
- Display time zones for time-based widgets, preferably UTC to avoid confusion between people in different timezones
- Default your time intervals and datapoint periods to the most common use case
- Do not plot too many points in your graphs -- can slowdown loading time and might reduce visibility of anomalies
- Annotate graphs with relevant alarm thresholds
  - Gives developers a quick glance to know if a metric might be violated soon, and can then plan and take actions ahead of time

### Pricing

50 dashboards are free, after that each dashboard costs $3 per month
