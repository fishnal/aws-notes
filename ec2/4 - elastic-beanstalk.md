# Elastic Beanstalk

- Takes uploaded code and provisions/deploys required resources for your application
- Resources can include EC2 instances, auto scaling, health-monitoring, and Elastic Load Balancing
- Useful for developers who aren't familiar with provisioning the correct environment and resources for an application.
- Works with a variety of platforms and programming languages
- Beanstalk itself is free to use; **however,** you must pay for any resources that are created/used (i.e. EC2 instances)

**Application Versioning**
- Beanstalk can manage application versions
- Each version typically points to an S3 bucket
- _NOTE_: "application versions" are not the same as "applications" (see below for more)

**Environment**
- An application version that has been deployed on AWS resources
- An environment contains **ALL** of the resources created by Beanstalk (not just the EC2 instance)

**Environment Configuration**
- Can configure what resources are provisioned for the environment

**Environment Tiers**
- Web Server environment - for a website, web app, or REST servers
- Worker environment - for all other use cases, such as long-running tasks

**Platform**
- Configures the OS, programming language, server type, and other components of Beanstalk

**Application**
- Collection of environments, environment configurations, and application versions
- An application can contain _multiple_ application versions

## General Workflow
1. Create your application (i.e. code it!)
2. Upload the version to Beanstalk
3. Launch the environment
4. Manage your environment as necessary
	- Deploying a new application version
	- Updating the environment itself, which will re-launch the environment (i.e. it might need to create/delete/modify resources)
