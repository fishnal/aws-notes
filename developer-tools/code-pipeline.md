# CodePipeline

Fully managed continuous delivery system

Features
- Automation of build, test, and release cycles
- Can setup manual approvals
- Pipeline history reports
- Pipeline status visualization

Used to orchestrate the CI/CD lifecycle by integrating with CodeCommit, CodeBuild, and CodeDeploy

## Pipeline Concepts

A **pipeline** consists of stages

A **stage** consists of actions

**Actions** within a stage can run in parallel or sequentially

A **transition** exists between stages

### Action Types

- Approval - a manual approval step
- Source - specify location of source code (i.e. S3 bucket, CodeCommit repo, other git repo)
- Build - specifies a build provider (i.e. CodeBuild)
- Test - specifies a test provider (i.e. CodeBuild, BlazeMeter)
- Deploy - specifies a deployment provider (i.e. CodeDeploy, CloudFormation, ECS, Elastic Beanstalk)
- Invoke - invoke a lambda directly from the pipeline

## Integration Options

Pipeline Action Types
- Most actions can integrate with another service; see [Action Types](#action-types)

CloudWatch Events for monitoring your pipelines
- Can setup SNS topics which can invoke lambda functions to do additional work within your pipeline
- By default, all pipeline are configured to use CloudWatch Events in order to automatically start when a new commit is detected in CodeCommit
	- This can be swapped for a _polling mechanism_
