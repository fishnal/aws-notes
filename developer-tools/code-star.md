# CodeStar

Simplifies process of setting up a CI/CD tool chain
- Manages all provisioning by leveraging **CloudFormation**

Composed of other AWS developer tools (as well as 3rd party tools)
- CodeCommit
- CodeBuild
- CodeDeploy
- CodePipeline

Features
- Preconfigured CI/CD workflow templates
- Dashboard visualization - unified, single-pane view of your workflow
	- Customizable with tiles
- Team membership management via IAM
	| Action                              | Owner | Contributor | Viewer |
	| ----------------------------------- | ----- | ----------- | ------ |
	| View project, dashboard, and status | YES   | YES         | YES    |
	| Modify access to project resources  | YES   | YES         | NO     |
	| Add/Remove team members             | YES   | NO          | NO     |
	| Delete project                      | YES   | NO          | NO     |
- Issue and Ticket Tracking
- Extensions - for integrating 3rd party services

## Provisioning Projects

A **CodeStar project** represents the AWS services that have been integrated together for your CI/CD workflow

There are a handful of project templates for common setups (i.e. Express NodeJS, Go Web App)

Provisioning is done via CloudFormation, **but has a pre-requisite**
- An IAM user with admin privileges must allow one-time creation of a new **service role** called "AWS CodeStar Service Role" with permissions to create your necessary CI/CD resources
