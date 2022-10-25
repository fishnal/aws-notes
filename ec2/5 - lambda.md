# Lambda

- Allows you to run code without managing EC2 instances
- AWS will manage all of the compute resources necessary.
- "Serverless" does not mean there isn't a server. There must be one, but the user does not have to worry about managing the server or it's resources.
- Only pay for when your code is running (as opposed to an EC2 instance, where you pay for when the server is running, regardless of how much traffic it gets)
- Supports many popular languages
- Lambdas execute when they are **triggered** from an event source

## Configuration
- Max execution timeout (how long lambda should run until it timesout)
- Required resources
- IAM role
- Handler name (name of the entry function in your code)

## Components
1. Lambda function - which is your compiled code
2. Event sources - AWS services that can be used to trigger the lambda
3. Trigger - an operation from an event source that causes the lambda to run
4. Downstream resources - resources that are required during the execution of your lambda (i.e. your lambda queries a DDB table, so the DDB table is considered a downstream resource)
5. Log streams
