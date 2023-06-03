# Step Functions

AWS Step Functions is a serverless orchestration service thats integrates with Lambda

You define a **state machine**, which is a visual workflow representation to more easily arrange and coordinate individual components of your distributed app.

Define your workflow with an **Amazon State Language** file
- Amazon State Language is a JSON-based proprietary language

## Types of States

1. Pass State - passes input straight to output; usually a debugging state
2. Task State - define a resource that you want to run (i.e. Lambda)
3. Choice State - given an input, choose an output based on conditions
4. Wait State - pause and wait for some time
5. Succeed State - successfuly termination of state machine
6. Fail State - failure termination of state machine with an error message and cause
7. Parallel State - executes a group of states concurrently, waiting for all branches to terminate; the result of all states is passed as output
8. Map State - perform tasks on a list of items; can be concurrent
	1. Like a `for-loop`

## Direct Integration with Services

Step Functions directly integrates with some services
- For example, you don't need a Lambda to perform a DDB operation. Instead, you can define the DDB operation in your state machine's task (this is cheaper and faster!)

## Other Features

Asynchronous Callbacks
- Overall, enables your workflows to be dynamic and resilient
- Can have a workflow that waits for a manager's approval
- Can utilize a 3rd party service (which may take hours or days to complete)

Nesting State Machines
- Can nest a child machine within a parent machine
- Great for repeatable patterns/workflows
