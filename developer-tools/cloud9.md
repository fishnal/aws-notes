# AWS Cloud9

Cloud-based IDE for writing and debugging code within browser
- Only requires an internet connection and a browser with modern Javasript engine
- Hosts the actual backend IDE on an EC2 instance
- Can configure the compute size of the underlying EC2 instance
- Features include
	- Themes, multiple cursors, keyboard bindings, syntax highlighting
	- Debugger, integrated terminal, real-time peer programming, keyboard bindings, IntelliSense, integration with Lambdas
- For peer programming, can control other team member access for read/write permissions
- Membership is handled by IAM
- Have `sudo` access in integrated terminal
	- AWS CLI is installed and is configured with temporary credentials
- Integration with Lambdas for developing lambda functions
- Has an internal file revision history for tracking quick changes
