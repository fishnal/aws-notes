ECS vs EKS
ECR

Fargate vs Lightsail

Elastic Beanstalk

cloudformation

RPO

S3 glacier
- There are two types of Expedited retrievals: On-Demand and Provisioned. On-Demand requests are similar to EC2 On-Demand instances and are available most of the time. Provisioned requests are guaranteed to be available when you need them, which is recommended for a DR plan.


inspector

disaster recovery methods
https://aws.amazon.com/blogs/publicsector/rapidly-recover-mission-critical-systems-in-a-disaster/
- backup and restore
- pilot light
	- A very small replica of critical systems is partially running but requires additional deployments before handling traffic.
- warm standby
- multi-site (complete copies of your system deployed elsewhere, most expensive)
