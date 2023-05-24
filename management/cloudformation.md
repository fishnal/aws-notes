# CloudFormation

Automates creating, updating, and deleting resources

Declaritvely define resources using standard languages

CloudFormation service is considered **Infrastructure as Code**, which means you can apply best practices for software development to your infrastructure provisioning
- Code versioning to track changes
- Rigorous testing
- Deploying through a CI/CD

Write CloudFormation **templates** either in JSON or YAML

CloudFormation takes in a template as input, and generates a **stack** as output
- **Stack** is a collection of resources that you can manage as a single unit
- Each stack has a unique name and a linked template
- Can delete an entire stack if the resourcfes aren't needed

**Stack policies** - policy document for defining what update actions can be performed on which resources
- If no stack policy is set (which is the default), then _all update actions_ are allowed on _all resources_
- Otherwise, if a stack policy is set, then all resources in the stack are protected by default. You must then explicitly allow updates on specific resources in the policy
- Can only define 1 stack policy er stack
- Stack policy applies to all users who attempt to update the stack
	- In other words, you cannot associate different stack policies with different users
- Stack policy applies only during stack updates; it does not prevent users from modifying the resources _outside of CloudFormation_

When CloudFormation is applying a stack, if any resource in the template can't be created, then CloudFormation **rolls back**
- During rollback, by default, all created resources will be destroyed
- With this default rollback behavior, stacks are **all or nothing**

Does not support all services

## Structure of Template

`AWSTemplateFormatVersion` - version of the CloudFormation template

`Description` - optional field

`Resources` - only required section where at least 1 resource must be declared; contains the following
- `Logical ID` - unique within template, used for referencing this resource in other parts of the template
	- Example: For an EC2 instance, your logical ID might be called `my-web-app-ec2`, but the **physical ID** might be `i-1234567890abcdef0`
- `Resource Type` - CloudFormation pre-defined code for the the resource (i.e. `AWS::EC2::Instance` is the code for creating an EC2 instance)
- `Resource Properties` additional options you specify for the resource (i.e. for an EC2 instance, can declare extra properties like Instance Type, ImageID)

`Parameters` - can pass values to resources during the stack creation
- Parameters can have default values
- Useful when your resource creation needs to be dynamic and choose certain values at run-time (i.e. the type of your EC2 instance)
- _Psuedo-parameters_ are pre-defined by AWS and do not need to be explicitly declared, for example the AWS account ID, or region.

`Mappings` - an object that contains maps (so the key is map name, and value is map itself)
- Each map is a simple key-value dictionary
- Useful when values might change based on the region that the resource is being deployed to
	- Example: AMI IDs are different across regions

`Conditions` - control what resources are created based on certain conditions

`Transform` - specify macros that you can use in your template

`Outputs` - can return resource information once the stack is created; outputs can be seen in the CloudFormation console or you can import them to another stack
- Example: Can output the public IP address of an EC2 instance created from your template

**Intrinsic Functions** - built-in functions
- `!Ref` - returns the value of a parameter or resource
	- For a parameter, it returns the parameter's value
	- For a resource, it returns the physical ID
- `!GetAtt` - returns the value of a resource attribute
- `!FindInMap` - find a value in a map given a key
- `!Join` - joins two values using a delimiter
