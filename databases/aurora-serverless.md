# Aurora Serverless

Elastic solution that autoscales the compute layer for an Aurora Serverless database
- Only bills you when the compute layer is in use
- Suited for variable or infrequent workloads

When provisioning an Aurora Serverless DB, you only need to specify lower and upper limits for capacity; you don't need to allocate instance sizes
- Capacity is measured in **ACUs (Aurora Capacity Units)**

Features
- Instances can be cold-booted within seconds
- Still uses the same self-healing, fault-tolerant storage layer as a regular Aurora database
- No need to add in read replicas; Aurora Serverless is designed to autoscale accordingly
- Has only **1 connection endpoint** (since it is serverless) used for BOTH read and write
- Can enable the **Web Service Data API**, only available for Aurora Serverless
    - API makes it easier to implement Lambdas that need to execute queries to the database (i.e. read or write)
    - API also available in AWS CLI
- Automatic, continuous backup with a default retention of 1 day (max 35 days)
    - Restores are performed to a new _serverless_ database cluster