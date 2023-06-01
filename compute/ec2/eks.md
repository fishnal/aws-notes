# Elastic Container Service for Kubernetes (EKS)

- Kubernetes is a container orchestration tool that is also **container-runtime agnostic**
- EKS manages Kubernetes for you, and allows you to run Kubernetes across your AWS infra
- Don't have to manage the **control plane**
- You only need to manage worker nodes

**Kubernetes Control Plane**
- Schedules containers onto nodes
	- NOTE: "scheduling" here refers to a decision process of placing containers on certain nodes (based off the node's compute requirements)
- Tracks state of all Kubernetes objects

**Worker Nodes**
- A worker machine in Kubernetes.
- Runs as an on-demand EC2 instances
- For each node, a specific AMI is used to ensure Docker and `kubelet` are installed with tighter security
- Once nodes are provisioned, they can connect to EKS via an endpoint

## Working with EKS
1. Create an IAM role that allows EKS to provision and configure resources. Attach the following policies to the role:
	1. `AmazonEKSServicePolicy`
	2. `AmazonEKSClusterPolicy`
2. Create an EKS Cluster VPC
3. Install `kubectl` and AWS-IAM Authenticator
4. Create the EKS Cluster, using the VPC details from step 2
5. Configure `kubectl` for EKS
6. Provision and configure worker nodes
7. Configure worker nodes to join your EKS cluster
