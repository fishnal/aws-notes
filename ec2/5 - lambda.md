# Lambda

- Allows you to run code without managing EC2 instances
- AWS will manage all of the compute resources necessary.
- "Serverless" does not mean there isn't a server. There must be one, but the user does not have to worry about managing the server or it's resources.
- Only pay for when your code is running (as opposed to an EC2 instance, where you pay for when the server is running, regardless of how much traffic it gets)
- Supports many popular languages
- Lambdas execute when they are **triggered** from an event source

## Security and Permissions
When importing code, AWS Lambda needs global read permissions to your deployment package.
- So in file permissions, the owner, group, and others must be able to read the files

Role Execution Policy - determines what resources the function has access to when executing

Function Policy - determines which AWS resources are allowed to invoke your function

## Configuration
- Max execution timeout (how long lambda should run until it timesout)
- Required resources
- IAM role
- Handler name (name of the entry function in your code)
- Max concurrency - how many invocations can occur at once

## Components
1. Lambda function - which is your compiled code
2. Event sources - AWS services that can be used to trigger the lambda
3. Trigger - an operation from an event source that causes the lambda to run
4. Downstream resources - resources that are required during the execution of your lambda (i.e. your lambda queries a DDB table, so the DDB table is considered a downstream resource)
5. Log streams

## Event Sources
Event sources can either be poll or push based
- Push based services will publish events and then invoke your lambda
- Poll based services are when lambda polls for a service, looking for a specific event (and when that event is found, the lambda is invoked)

**Event source mapping** configures the link between an event source and your lambda
- For push based services, the mapping is maintained in the event source
- For poll based services, the mapping is held within your lambda

## Sync/Async Invocations
Poll-based events are ALWAYS synchronous invocations

Push-based events, the invocation type (sync vs async) varies based on service

## CloudWatch
- Invocations - # of times lambda was invoked
- Errors - # of failed invocations
- DeadLetterErrors - # of times lambda fails to write to DLQ
- Duration - how long (in ms) lambda took to complete
- Throttles - # of times lambda was invoked and throttled due to limit of concurrency
- IteratorAge - (only for stream-based invocations) how long (in ms) it took to receive a batch of records to the time of the last record written to the stream
- ConcurrentExecutions - combined metric for all lambdas plus custom concurrency limits; gives # of lambdas with a custom concurrency limit that were executed at a given time
- UnreservedConcurrenctExecutions - similar to "ConcurrentExecutions" except that it's only in regard to functions _without a custom concurrency limit_
