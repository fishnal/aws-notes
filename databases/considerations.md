## When to use Relational databases

Great for
- Structured data
- Data with lots of relationships
- Relationships that must be enforced by a strict schema
- ACID compliance

### Do you need an OLAP or OLTP database?

Redshift is good for OLAP
- Best when data is well-defined: you know the structure, schema, and volume
- Keep in mind that _Redshift is billed based on server uptime_, so how often your Redshift database is used is also an important consideration

RDS is good for OLTP

### Which database engine for RDS?

Based on
1. Team skills
2. Developer preference
3. Frameworks and languages that you will use (i.e. Python is very popular, and as a result has more libraries and general community support)
4. Existing database technology (i.e. you want to migrate on-premise database to the cloud). Must consider that
    - RDS does not enable access to OS or root
    - Certain RDS engines don't support all features (i.e. MySQL RDS engine may lack some features to MySQL on-premise)

If needed, you can use **RDS Custom**, which runs either Oracle or SQL Server instances
- Can customize patching, high availability configs, and install 3rd party apps
- Allows host-level access and modification of file systems
- RDS will still take care of setting it up, operating it, and scaling it for you. Because of this, RDS Custom is **semi-managed**
- Limitations
    - For RDS Custom Oracle
        - Cannot enable Kerberos authentication
        - Cannot turn on storage auto scaling
        - Cannot use Oracle multi-tenant architecture
    - For RDS Custom SQL Server
        - Cannot use read replicas
        - Cannot modify storage size after creating database
        - Cannot create multi-AZ deployment
- If these limitations are a problem for you, then **consider hosting your database on an EC2 instance**

Scalability Requirements
- MariaDB, MySQL, PostgreSQL, and Oracle all support 64 TiB storage
- SQL Server only supports 16 TiB storage

### When to use Aurora?

Great for relational, OLTP workloads that operate at a massive scale with consistent traffic patterns
- If you don't have consistent traffic but still require a large scale database for these kind of worloads, consider **Aurora Serverless**

Max storage limitation for MySQL and PostgreSQL for Aurora DB is 128TB. Other engines have up to 64TB.

## When to use Non-Relational databases