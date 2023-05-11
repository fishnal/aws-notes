# AWS Serverless Application Model (SAM)

Open source framework for building serverless apps in AWS

Define and deploy serverless resources (lambdas, API gateways, databases), using YAML templates
- During deployment process, SAM will transform YAML templates into CloudFormation templates

SAM CLI allows you to build, test, and debug SAM applications locally
- `sam build` - builds app locally, transforms YAML SAM templates into CloudFormation templates
- `sam package` - uploads CloudFormation templates and project code to S3
- `sam deploy` - creates and executes CloudFormation change set, which deploys your app
