# CodeDeploy

Fully managed deployment service

Uses an **agent-based method** for performing deployments on servers
- Agent communicates with CodeDeploy using HTTPS protocol, polling for code deployment updates
- Agent **is not** needed for serverless deployments

Uses an `AppSpec` file where you define installation steps/commands to get your artifact up and running
- Can be written in JSON or YAML, depending on the deployment endpoint.
	- If deploying a lambda function, then can use either JSON or YAML
	- If deploying to EC2 instance or on-premise server, then must use YAML

**Deployment group** is a group of servers that have CodeDeploy agent installed and the servers that you want to deploy to
- Can be configured by specifying an auto-scaling group, EC2 instances with tags, and/or on-premise servers with tags

Provisioning a new **deployment bridge**
- Needs a deployment group
- Needs a revision/version of the software you want to deploy

Can control the **speed of deployments** in terms of how many instances are updated at once
- In the case of Lambdas, you can control **how traffic is rerouted** incrementally from the old Lambda to the new one

Can create **deployment configurations**, or use an existing preset

## Integration

CloudWatch Events - for monitoring the state of an instance or deployment

Deployment Group Triggers - can register an SNS topic to receive deployment event messages
- Can then subscribe to the topic to perform other actions

`AppSpec` file - ability to hook in commands and custom scripts before/after particular deployment events
