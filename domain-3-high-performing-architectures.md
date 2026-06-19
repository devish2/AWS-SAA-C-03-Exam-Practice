# Domain 3: Design High-Performing Architectures (24 Questions)

---

### Question 1
A global news website serves the same static images and videos to millions of users worldwide. Users in regions far from the origin server experience slow load times. What should be implemented to reduce latency for these users?

A) Increase the size of the origin EC2 instance
B) Amazon CloudFront to cache content at edge locations close to users
C) Move the origin server to a single, centrally located Region
D) Increase the number of read replicas

<details><summary>Show Answer</summary>

**Answer: B**

CloudFront caches content at edge locations distributed globally, so users are served from a nearby edge location rather than round-tripping to the origin, dramatically reducing latency for geographically distributed audiences. Increasing origin instance size (A) doesn't address network distance/latency.
</details>

---

### Question 2
An application performs the same expensive, repeated read queries against a relational database, causing high latency under load. The data changes infrequently. What should be added to reduce database load and improve response times?

A) A read replica only
B) Amazon ElastiCache (Redis or Memcached) as a caching layer in front of the database
C) Increasing the database instance size
D) Switching to a NoSQL database with no other changes

<details><summary>Show Answer</summary>

**Answer: B**

ElastiCache provides an in-memory cache that can store the results of expensive, repeated queries; since the underlying data changes infrequently, a cache-aside pattern dramatically cuts database load and serves repeated reads with sub-millisecond latency. A read replica (A) still hits a database engine for every query, just on different hardware.
</details>

---

### Question 3
A DynamoDB-backed application needs microsecond-level read latency for an extremely read-heavy, repeatedly-accessed dataset. What should be added in front of DynamoDB?

A) DynamoDB Accelerator (DAX)
B) An RDS read replica
C) S3 Transfer Acceleration
D) AWS Global Accelerator

<details><summary>Show Answer</summary>

**Answer: A**

DAX is a fully managed, in-memory cache purpose-built for DynamoDB, providing microsecond response times for read-heavy workloads without requiring application logic changes beyond pointing to the DAX endpoint. RDS read replicas (B) are unrelated to DynamoDB.
</details>

---

### Question 4
A company wants to choose the right EBS volume type for a workload requiring the highest possible IOPS for a transactional database with unpredictable, latency-sensitive I/O. Which volume type is most appropriate?

A) Cold HDD (sc1)
B) Throughput Optimized HDD (st1)
C) Provisioned IOPS SSD (io2 / io2 Block Express)
D) General Purpose SSD (gp2) only

<details><summary>Show Answer</summary>

**Answer: C**

io2 (and io2 Block Express) volumes are designed for I/O-intensive, latency-sensitive transactional workloads requiring sustained, high, and consistent IOPS, with the ability to provision IOPS independently of volume size. HDD-based volumes (A, B) are designed for throughput-oriented, sequential workloads like big data or log processing, not low-latency random I/O.
</details>

---

### Question 5
A company runs a batch analytics workload that reads large, sequential files and is throughput-sensitive rather than IOPS-sensitive, and the workload is not latency-critical. Which EBS volume type provides the best price/performance for this use case?

A) Provisioned IOPS SSD (io2)
B) Throughput Optimized HDD (st1)
C) General Purpose SSD (gp3)
D) Instance store only

<details><summary>Show Answer</summary>

**Answer: B**

st1 volumes are optimized for large, sequential, throughput-intensive workloads like big data and log processing, offering a lower cost per GB than SSD options for this access pattern. io2 (A) is over-provisioned (and overpriced) for a throughput-bound, non-latency-critical batch job.
</details>

---

### Question 6
An application's compute layer needs to process a highly variable, unpredictable workload of short-lived tasks (a few seconds each), and the company wants to avoid managing servers entirely while only paying for actual compute time used. What should they use?

A) A fleet of always-on EC2 instances sized for peak load
B) AWS Lambda
C) A single large EC2 instance running a queue consumer continuously
D) Amazon EC2 Reserved Instances

<details><summary>Show Answer</summary>

**Answer: B**

Lambda is serverless and billed per invocation/duration, automatically scaling to handle highly variable, bursty, short-lived workloads with zero server management — a strong fit for unpredictable traffic where paying for idle always-on capacity (A, D) would be wasteful.
</details>

---

### Question 7
A company runs a tightly-coupled HPC (high-performance computing) workload across many EC2 instances that requires very low network latency and high throughput between instances. What EC2 placement strategy should be used?

A) Spread placement group
B) Cluster placement group
C) Partition placement group
D) Launching instances in different AZs for diversity

<details><summary>Show Answer</summary>

**Answer: B**

A cluster placement group packs instances close together within a single AZ on the same underlying low-latency, high-throughput network fabric, ideal for tightly-coupled HPC workloads. Spread placement groups (A) prioritize fault isolation across distinct hardware, which is the opposite goal here. Different AZs (D) would increase, not decrease, inter-instance latency.
</details>

---

### Question 8
A company wants to ensure consistent, low-latency global routing of TCP/UDP traffic to the optimal AWS endpoint (not just caching static content), supporting both failover and load balancing across multiple Regions for a gaming application. What should be used?

A) Amazon CloudFront
B) AWS Global Accelerator
C) Amazon Route 53 latency-based routing alone
D) Amazon S3 Transfer Acceleration

<details><summary>Show Answer</summary>

**Answer: B**

Global Accelerator uses the AWS global network and anycast static IPs to route TCP/UDP traffic to the optimal, healthy endpoint across Regions, providing fast failover and consistent performance for non-HTTP, latency-sensitive workloads like gaming — a different use case than CloudFront's HTTP/S content caching (A).
</details>

---

### Question 9
An application uploads large files directly from end users around the world into an S3 bucket in `us-east-1`. Users far from `us-east-1` experience slow upload speeds. What feature can improve this without changing the bucket's Region?

A) S3 Cross-Region Replication
B) S3 Transfer Acceleration
C) S3 Intelligent-Tiering
D) Increasing the EC2 instance size of the application servers

<details><summary>Show Answer</summary>

**Answer: B**

S3 Transfer Acceleration routes uploads through the nearest CloudFront edge location and over the optimized AWS backbone network to the bucket's Region, significantly speeding up uploads for geographically distant users without requiring the bucket to move.
</details>

---

### Question 10
A company's relational database is experiencing connection exhaustion because their serverless application (Lambda) opens a new database connection on every invocation, and concurrency spikes during peak load. What should be used to manage and pool these connections efficiently?

A) Increase the RDS instance size only
B) Amazon RDS Proxy
C) Switch from RDS to S3
D) Reduce Lambda timeout to 1 second

<details><summary>Show Answer</summary>

**Answer: B**

RDS Proxy pools and shares database connections, sitting between the application (e.g., Lambda) and the database, preventing connection exhaustion during traffic spikes and improving failover handling, without requiring application-level connection management logic.
</details>

---

### Question 11
A company is choosing a database for a new application that requires single-digit millisecond performance at any scale, with a flexible schema and a simple key-based access pattern (no complex joins). Which database is the best fit?

A) Amazon RDS for PostgreSQL
B) Amazon DynamoDB
C) Amazon Redshift
D) Amazon RDS for MySQL with read replicas

<details><summary>Show Answer</summary>

**Answer: B**

DynamoDB is a fully managed NoSQL database purpose-built for consistent, single-digit-millisecond performance at any scale, with a flexible (schemaless) item structure suited to key-based access patterns. Redshift (C) is a data warehouse for analytical/OLAP queries, not low-latency transactional access. Relational options (A, D) involve schema and join overhead not needed here.
</details>

---

### Question 12
A company runs an analytics workload that needs to run complex SQL queries across petabytes of structured data for business intelligence reporting. Which AWS service is purpose-built for this?

A) Amazon DynamoDB
B) Amazon Redshift
C) Amazon ElastiCache
D) Amazon S3 Glacier

<details><summary>Show Answer</summary>

**Answer: B**

Redshift is a managed petabyte-scale data warehouse optimized for complex analytical (OLAP) SQL queries across very large datasets, using columnar storage and massively parallel processing — exactly the BI/reporting use case described. DynamoDB (A) is optimized for transactional key-value access, not complex analytical joins/aggregations.
</details>

---

### Question 13
A company's API Gateway-backed REST API returns the same response for a given set of query parameters most of the time, and the backend Lambda function is the main cost and latency driver. What feature can reduce both latency and backend load for repeated identical requests?

A) API Gateway caching
B) Increasing Lambda memory only
C) Removing the API Gateway and calling Lambda directly from clients
D) AWS WAF

<details><summary>Show Answer</summary>

**Answer: A**

API Gateway has a built-in caching feature that stores responses for a configurable TTL, so identical subsequent requests are served from the cache without invoking the backend Lambda function at all, reducing both latency and backend invocation costs.
</details>

---

### Question 14
A company wants to run containerized microservices without managing the underlying EC2 instances at all — no patching, no capacity planning for the hosts. Which compute option fits this requirement?

A) Amazon ECS or EKS on EC2 launch type
B) Amazon ECS or EKS with the Fargate launch type
C) Self-managed Kubernetes on EC2
D) AWS Lambda only, with no containers

<details><summary>Show Answer</summary>

**Answer: B**

Fargate is a serverless compute engine for containers — it runs ECS or EKS tasks/pods without the customer provisioning, patching, or managing any EC2 instances, fully abstracting away the underlying host infrastructure. The EC2 launch type (A) still requires managing the underlying instances.
</details>

---

### Question 15
A company wants its DynamoDB table to scale read and write throughput automatically based on actual traffic, without manually provisioning capacity units or risking throttling during unpredictable spikes. What capacity mode should they use?

A) Provisioned capacity mode with no auto scaling
B) DynamoDB On-Demand capacity mode
C) A fixed, manually-set high provisioned capacity at all times
D) Switching to Amazon RDS instead

<details><summary>Show Answer</summary>

**Answer: B**

On-Demand capacity mode automatically scales to handle workloads of virtually any throughput level instantly, with pay-per-request pricing, removing the need to predict or manually manage capacity for unpredictable traffic patterns.
</details>

---

### Question 16
A company is designing a DynamoDB table to track user activity events, and they're concerned about a "hot partition" problem where most traffic concentrates on a small subset of partition key values, degrading performance. What is the best practice to avoid this?

A) Use a low-cardinality partition key, like a fixed `status` value, shared by most items
B) Choose a partition key with high cardinality and even access distribution, such as a composite of user ID and a random/sharded suffix
C) Always use a single global counter as the partition key
D) Avoid partition keys entirely and rely on a single table-wide index

<details><summary>Show Answer</summary>

**Answer: B**

A well-distributed, high-cardinality partition key spreads requests evenly across the underlying partitions, avoiding the hot-partition throttling problem. A low-cardinality key (A), like a shared status value, concentrates traffic onto very few partitions and is the classic cause of hot partitions.
</details>

---

### Question 17
A company's web application experiences a sudden 10x traffic spike during a marketing event and needs the EC2 fleet behind the ALB to scale out quickly enough to maintain performance. Which Auto Scaling configuration best supports rapid, proportional scale-out for sudden demand increases?

A) Simple scaling with a long cooldown period
B) Target tracking scaling policy on a relevant metric (e.g., ALB request count per target), supplemented by predictive scaling if the traffic pattern is known in advance
C) No scaling policy — manually launch instances when alerted
D) Scheduled scaling only, set once a year

<details><summary>Show Answer</summary>

**Answer: B**

Target tracking automatically adjusts capacity to maintain a target metric value and reacts continuously as load changes, and predictive scaling can pre-scale ahead of a known event using ML-forecasted patterns, both of which respond faster and more accurately than simple scaling with long cooldowns or manual intervention.
</details>

---

### Question 18
A company needs to run a relational database with the performance of high-end commercial databases but at lower cost, and wants MySQL/PostgreSQL compatibility along with storage that automatically scales up to 128 TB as needed. Which AWS database service fits best?

A) Amazon RDS for SQL Server
B) Amazon Aurora
C) Amazon DynamoDB
D) Amazon Redshift

<details><summary>Show Answer</summary>

**Answer: B**

Aurora is AWS's MySQL- and PostgreSQL-compatible relational engine, built for higher throughput than standard RDS engines, with storage that auto-scales up to 128 TB and is designed to deliver commercial-database-class performance at open-source-compatible cost.
</details>

---

### Question 19
A company wants to reduce the latency experienced by users querying an Amazon RDS database located in a single Region, for users distributed across multiple continents, without re-architecting to a NoSQL database. What read-scaling option helps reduce read latency for geographically distant users?

A) Amazon RDS Cross-Region Read Replicas placed closer to distant user bases
B) Increasing the primary instance's storage size
C) S3 Transfer Acceleration
D) Amazon Redshift

<details><summary>Show Answer</summary>

**Answer: A**

Cross-region read replicas let read traffic be served from a replica located geographically closer to a distant user population, reducing round-trip latency for reads while keeping the relational engine and schema unchanged. (Writes would still go to the primary Region.)
</details>

---

### Question 20
A company's EFS-backed application requires the highest possible throughput for a workload with many concurrent clients (hundreds of EC2 instances accessing the same file system in parallel). Which EFS performance mode should be selected?

A) General Purpose mode
B) Max I/O mode
C) Provisioned Throughput mode only, with General Purpose
D) Single-AZ mode

<details><summary>Show Answer</summary>

**Answer: B**

Max I/O mode is designed to scale to higher levels of aggregate throughput and operations per second for highly parallelized workloads with many concurrent clients, at the cost of slightly higher per-operation latency compared to General Purpose mode (A), which is optimized for latency-sensitive workloads with lower concurrency.
</details>

---

### Question 21
A company's serverless application experiences "cold start" latency spikes for an infrequently-invoked but latency-sensitive Lambda function used in a customer-facing checkout flow. What feature can reduce this cold start impact?

A) Increasing the Lambda function's timeout
B) Provisioned Concurrency
C) Switching the function's IAM role
D) Disabling X-Ray tracing

<details><summary>Show Answer</summary>

**Answer: B**

Provisioned Concurrency keeps a specified number of execution environments pre-initialized and ready to respond immediately, eliminating cold-start latency for the configured concurrency level — directly addressing latency-sensitive, infrequently-invoked functions.
</details>

---

### Question 22
A company wants to analyze only a small subset of columns from large CSV/Parquet objects stored in S3, without downloading and processing the entire object in the application, to reduce both data transfer and compute time. What S3 feature helps with this?

A) S3 Select
B) S3 Versioning
C) S3 Lifecycle policies
D) S3 Same-Region Replication

<details><summary>Show Answer</summary>

**Answer: A**

S3 Select lets applications retrieve only the specific data needed from an object using simple SQL-like expressions, evaluated server-side, dramatically reducing the amount of data transferred and the processing needed on the client/application side, compared to retrieving the full object.
</details>

---

### Question 23
A company's web tier needs to scale based on a custom application-level metric (e.g., number of active WebSocket connections per instance) rather than a generic metric like CPU utilization. How can Auto Scaling be configured to use this?

A) Auto Scaling only supports CPU-based scaling
B) Publish the custom metric to CloudWatch and create a target tracking or step scaling policy based on that custom metric
C) Manually scale based on the custom metric using a spreadsheet
D) Use Reserved Instances instead of Auto Scaling

<details><summary>Show Answer</summary>

**Answer: B**

Auto Scaling target tracking and step scaling policies can be based on any CloudWatch metric, including custom metrics published by the application (e.g., via the CloudWatch PutMetricData API), allowing scaling decisions to reflect application-specific load indicators beyond just CPU or network.
</details>

---

### Question 24
A company is choosing between Standard and Express workflows in AWS Step Functions for an order-processing pipeline that runs millions of very short, high-volume executions per day, where exactly-once semantics aren't critical but high throughput and low cost per execution are. Which workflow type is more appropriate?

A) Standard Workflows
B) Express Workflows
C) Neither — use only Lambda with no orchestration
D) Amazon SWF (Simple Workflow Service) as a direct replacement

<details><summary>Show Answer</summary>

**Answer: B**

Express Workflows are optimized for high-volume, short-duration (up to 5 minutes), high-throughput event-processing workloads with at-least-once execution semantics, priced per execution/duration — a better fit for millions of short executions per day than Standard Workflows (A), which prioritize exactly-once semantics and long-running durability (up to a year) at higher cost per execution.
</details>

---

[← Back to README](./README.md)
