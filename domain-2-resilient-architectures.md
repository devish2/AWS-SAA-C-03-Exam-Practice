# Domain 2: Design Resilient Architectures (26 Questions)

---

### Question 1
A company runs a relational database on a single EC2 instance and experiences downtime whenever that instance fails. They want a managed solution that automatically fails over to a standby in a different Availability Zone with minimal manual intervention. What should they use?

A) Amazon RDS in Multi-AZ deployment mode
B) A single Amazon RDS instance with automated backups
C) An EC2 instance with a cron job that creates nightly snapshots
D) Amazon S3 with versioning

<details><summary>Show Answer</summary>

**Answer: A**

RDS Multi-AZ automatically provisions and maintains a synchronous standby replica in a different AZ and performs automatic failover (updating the DNS endpoint) if the primary becomes unavailable, with no manual intervention required. A single-AZ instance (B) has no automatic failover target.
</details>

---

### Question 2
An application's read-heavy reporting workload is overwhelming the primary database, causing slow writes for the main application. The architect wants to offload read traffic without affecting write availability. What should be implemented?

A) Vertical scaling of the primary instance only
B) Amazon RDS Read Replicas for the reporting workload
C) Deleting old data to reduce load
D) Switching to a single larger EC2 instance running a self-managed database

<details><summary>Show Answer</summary>

**Answer: B**

Read replicas asynchronously replicate from the primary and can serve read-only traffic (like reporting queries), offloading that load from the primary instance so write performance isn't impacted. Vertical scaling (A) alone doesn't separate read and write workloads.
</details>

---

### Question 3
A company wants its web application to automatically add EC2 instances when CPU utilization exceeds 70% and remove them when it drops below 30%, while always keeping a minimum of 2 instances running across multiple AZs for resilience. What should be configured?

A) A single large EC2 instance with no scaling
B) An Auto Scaling group with target tracking scaling policies, spanning multiple AZs, with a minimum capacity of 2
C) Manually launching instances during business hours
D) An ELB with no Auto Scaling group attached

<details><summary>Show Answer</summary>

**Answer: B**

An Auto Scaling group with a target tracking policy automatically adds/removes instances based on the chosen metric (like CPU), while the minimum capacity setting and multi-AZ subnet configuration ensure baseline resilience even during low load. Without an ASG (D), the ELB has no mechanism to scale capacity.
</details>

---

### Question 4
A company's RPO (Recovery Point Objective) is 5 minutes and RTO (Recovery Time Objective) is near-zero for a critical application. They need a disaster recovery strategy across two AWS Regions that meets this. Which DR strategy is appropriate?

A) Backup and Restore
B) Pilot Light
C) Warm Standby
D) Multi-Site Active-Active

<details><summary>Show Answer</summary>

**Answer: D**

Multi-Site Active-Active runs full production capacity in two (or more) regions simultaneously, so failover is near-instantaneous (near-zero RTO) and data loss is minimal since both sites are continuously processing/replicating. Backup and Restore (A) has the highest RTO/RPO of the DR strategies. Pilot Light (B) and Warm Standby (C) have lower cost but require some scale-up time, giving higher RTO than near-zero.
</details>

---

### Question 5
A company wants the lowest-cost disaster recovery option for a non-critical application where several hours of downtime (RTO) and up to 24 hours of data loss (RPO) are acceptable in a true disaster. What strategy fits best?

A) Multi-Site Active-Active
B) Backup and Restore
C) Hot standby in a second region with full capacity running 24/7
D) Synchronous cross-region replication

<details><summary>Show Answer</summary>

**Answer: B**

Backup and Restore is the lowest-cost DR strategy: regular backups (e.g., to S3, possibly cross-region) are restored only when disaster strikes, accepting longer RTO/RPO in exchange for minimal standing infrastructure cost. The other options keep resources running continuously, which costs significantly more and isn't justified for a non-critical workload.
</details>

---

### Question 6
A globally distributed application needs DNS-based failover so that if the primary region's health check fails, traffic is automatically routed to a healthy secondary region. What should be configured?

A) Amazon Route 53 with a failover routing policy and health checks
B) A single A record pointing to one region only
C) An Application Load Balancer with cross-zone load balancing
D) AWS Direct Connect

<details><summary>Show Answer</summary>

**Answer: A**

Route 53 failover routing policies use health checks against the primary endpoint and automatically shift DNS responses to the secondary record if the primary becomes unhealthy — this is the standard mechanism for region-level DNS failover. An ALB (C) only load-balances within its own region/AZs, not across separate regional deployments.
</details>

---

### Question 7
A photo-sharing application stores user-uploaded images in S3 and needs durability protection against the accidental overwrite or deletion of objects, allowing recovery of any previous version of a file. What S3 feature should be enabled?

A) S3 Transfer Acceleration
B) S3 Versioning
C) S3 Requester Pays
D) S3 Select

<details><summary>Show Answer</summary>

**Answer: B**

S3 Versioning keeps every version of an object when it's overwritten or "deleted" (deletion just adds a delete marker), allowing any previous version to be restored. Transfer Acceleration (A) speeds up uploads over long distances and has no relation to durability/recovery.
</details>

---

### Question 8
A company wants its application's session state to survive the failure or scale-in of any individual EC2 instance, so users aren't logged out when Auto Scaling terminates an instance. What architecture should be used?

A) Store session state in memory on each EC2 instance (sticky sessions only)
B) Store session state in a shared, external data store such as Amazon ElastiCache (Redis) or DynamoDB
C) Store session state in a local file on each instance's EBS volume
D) Disable Auto Scaling to avoid instance termination

<details><summary>Show Answer</summary>

**Answer: B**

Externalizing session state to a shared store like ElastiCache or DynamoDB makes the application tier stateless — any instance can serve any request, and termination/replacement of an instance has no impact on the user's session. Local in-memory or on-disk storage (A, C) ties session data to a single instance, which breaks on termination.
</details>

---

### Question 9
A company processes orders through a chain of microservices. They want to ensure that if a downstream service is temporarily unavailable, order messages aren't lost and are retried automatically, decoupling the producer from the consumer's availability. What should be used between the services?

A) Direct synchronous HTTP calls only
B) Amazon SQS as a buffer between producer and consumer
C) A shared EBS volume
D) Storing the order directly in the consumer's local database

<details><summary>Show Answer</summary>

**Answer: B**

SQS decouples producers and consumers: messages persist in the queue until successfully processed, so a temporary outage in the downstream consumer doesn't cause message loss — the consumer simply catches up once it's healthy again. Direct synchronous calls (A) fail immediately if the downstream service is down, with no buffering.
</details>

---

### Question 10
A company wants messages that repeatedly fail processing (after several retry attempts) to be automatically moved aside for investigation, rather than blocking the queue or being silently dropped. What should be configured?

A) A second, unrelated SQS queue manually checked each day
B) An SQS Dead-Letter Queue (DLQ) with a redrive policy
C) Deleting failed messages immediately
D) Increasing the visibility timeout indefinitely

<details><summary>Show Answer</summary>

**Answer: B**

A Dead-Letter Queue, configured via a redrive policy with a maximum receive count, automatically captures messages that have failed processing too many times, isolating them for investigation without blocking the main queue or silently losing them.
</details>

---

### Question 11
A company runs a stateful application on EC2 that requires a shared, POSIX-compliant file system accessible concurrently by multiple instances across several Availability Zones, with automatic scaling of storage capacity. What should be used?

A) Amazon EBS (single volume attached to multiple instances)
B) Amazon EFS
C) Instance store (ephemeral storage)
D) Amazon S3 mounted as a local file system

<details><summary>Show Answer</summary>

**Answer: B**

EFS is a fully managed, elastic NFS file system that can be mounted concurrently by many EC2 instances across multiple AZs, automatically scaling storage up and down. Standard EBS volumes (A) can generally only attach to one instance at a time (io1/io2 Multi-Attach has significant limitations and isn't a general-purpose shared file system). Instance store (C) is ephemeral and instance-local.
</details>

---

### Question 12
A company's critical application depends on a third-party payment API that occasionally has brief outages. The architect wants the application to handle these gracefully — retrying with increasing delays rather than hammering the failing service. What pattern should be implemented?

A) Immediate, unlimited retries in a tight loop
B) Exponential backoff with jitter
C) Ignoring failures silently
D) A fixed 1-second retry with no limit

<details><summary>Show Answer</summary>

**Answer: B**

Exponential backoff with jitter increases the wait time between retries (with randomization to avoid synchronized retry storms), reducing load on a struggling downstream service while still recovering automatically once it's healthy — this is the standard resilience pattern recommended by AWS SDKs. A tight retry loop (A) can worsen an outage.
</details>

---

### Question 13
A media company wants to replicate objects uploaded to an S3 bucket in `us-east-1` to a bucket in `eu-west-1` automatically and continuously, for disaster recovery purposes. What feature should be configured?

A) S3 Lifecycle policies
B) S3 Cross-Region Replication (CRR)
C) Manual nightly `aws s3 sync` jobs
D) S3 Select

<details><summary>Show Answer</summary>

**Answer: B**

Cross-Region Replication automatically and asynchronously copies new objects (and optionally existing ones) from a source bucket to a destination bucket in a different Region, which is the native, managed way to achieve this for DR purposes. Manual sync jobs (C) add operational overhead and aren't real-time.
</details>

---

### Question 14
A company wants its DynamoDB table to remain available for both reads and writes in multiple AWS Regions simultaneously, with changes in one region automatically propagating to the others, to support a globally distributed user base with low latency. What feature should be used?

A) DynamoDB Streams alone
B) DynamoDB Global Tables
C) DynamoDB on-demand capacity mode
D) A single-region table accessed cross-region via VPC peering

<details><summary>Show Answer</summary>

**Answer: B**

DynamoDB Global Tables provide fully managed, multi-Region, multi-active replication, so applications can read and write to the table in whichever region is closest to the user, with changes propagated to all other regions automatically. Streams (A) is a building block Global Tables use internally, but isn't itself the multi-region replication feature.
</details>

---

### Question 15
An application is deployed across two Availability Zones behind an Application Load Balancer. To ensure the ALB only routes traffic to instances that are actually able to serve requests (not just "running"), what should be configured?

A) Nothing — the ALB automatically assumes all running instances are healthy
B) ALB health checks against a specific application endpoint (e.g., `/health`)
C) A CloudWatch alarm that emails the team when an instance is slow
D) Removing the unhealthy AZ from the VPC entirely

<details><summary>Show Answer</summary>

**Answer: B**

ALB health checks periodically send requests to a configured path on each target; targets that fail the check are marked unhealthy and automatically removed from the routing rotation until they pass again, ensuring traffic only reaches instances genuinely able to serve requests — not merely instances that are powered on.
</details>

---

### Question 16
A company is migrating an on-premises application to AWS and wants the underlying infrastructure (VPC, subnets, EC2 instances, security groups) to be reproducible, version-controlled, and consistently deployable across dev, staging, and production environments. What should they use?

A) Manually clicking through the console for each environment
B) AWS CloudFormation (or an equivalent Infrastructure-as-Code tool) with templates stored in version control
C) AMIs only, with manual configuration after launch
D) A shared spreadsheet documenting manual steps

<details><summary>Show Answer</summary>

**Answer: B**

CloudFormation lets infrastructure be defined as code, version-controlled, and deployed repeatably and consistently across multiple environments via stacks — eliminating configuration drift and manual error that come with clicking through the console each time.
</details>

---

### Question 17
A company needs to migrate 200 TB of on-premises data to S3 for a one-time bulk migration, and their internet connection would take several weeks to transfer that volume. What is the most appropriate solution?

A) AWS Snowball (or Snowball Edge) device shipped to their data center
B) Increasing their internet bandwidth temporarily
C) AWS Direct Connect set up that same day
D) Uploading via the S3 console in small batches over several weeks

<details><summary>Show Answer</summary>

**Answer: A**

For large one-time bulk transfers where network transfer would take too long, AWS Snowball physically ships a storage device to the customer's site; data is loaded locally and the device is shipped back to AWS for ingestion — far faster than trying to push 200 TB over a constrained link. Direct Connect (C) takes weeks to provision and is meant for ongoing connectivity, not same-day bulk transfer.
</details>

---

### Question 18
A company's application architecture currently has a single point of failure: all traffic passes through one NAT Gateway in one Availability Zone. If that AZ fails, all outbound internet access from private subnets in other AZs is lost. How should this be fixed?

A) Leave it as is since NAT Gateways are inherently highly available
B) Deploy a NAT Gateway in each Availability Zone and route each AZ's private subnet traffic through the NAT Gateway in the same AZ
C) Replace the NAT Gateway with a single NAT instance
D) Remove the NAT Gateway entirely

<details><summary>Show Answer</summary>

**Answer: B**

A NAT Gateway is itself redundant within its own AZ, but a single NAT Gateway is still an AZ-level single point of failure for the whole VPC. Best practice is one NAT Gateway per AZ, with each AZ's private subnet route table pointing to the NAT Gateway in that same AZ, so an AZ failure only affects that AZ's outbound traffic, not the whole application.
</details>

---

### Question 19
A company wants to decouple the ordering of operations so that an image-processing fleet only ever scales based on the depth of a work queue, rather than being directly invoked by the upload service. What two services would typically be combined for this pattern?

A) Amazon S3 and Amazon Route 53
B) Amazon SQS and an Auto Scaling group with a scaling policy based on queue depth (e.g., via a custom CloudWatch metric)
C) AWS Direct Connect and AWS Transit Gateway
D) Amazon CloudFront and AWS WAF

<details><summary>Show Answer</summary>

**Answer: B**

This is the classic queue-based decoupling/auto scaling pattern: work items go into an SQS queue, and an Auto Scaling group scales the worker fleet up or down based on a metric like `ApproximateNumberOfMessagesVisible`, fully decoupling producer load from consumer capacity.
</details>

---

### Question 20
A company runs a fleet of EC2 instances across 3 Availability Zones behind an ALB, with an Auto Scaling Group configured to keep 6 instances running (2 per AZ). One entire AZ becomes unavailable. What happens to application availability, assuming the ASG and ALB are correctly configured for multi-AZ?

A) The whole application goes down because one AZ failed
B) The ASG and ALB continue serving traffic using the healthy instances in the remaining 2 AZs, and the ASG can launch replacement instances in the healthy AZs to restore desired capacity
C) Manual intervention is required to detect the AZ failure and reroute traffic
D) All instances in all AZs are terminated automatically

<details><summary>Show Answer</summary>

**Answer: B**

This is the core benefit of multi-AZ resilience: the ALB stops routing to unhealthy targets in the failed AZ, traffic continues to flow to the healthy AZs, and the Auto Scaling group automatically launches replacement capacity in the remaining healthy AZs to maintain the desired count — all without manual intervention.
</details>

---

### Question 21
A financial services company needs synchronous, zero-data-loss replication between their primary database and a standby for a mission-critical transactional system within a single Region. Which RDS feature provides this?

A) RDS Read Replica (asynchronous)
B) RDS Multi-AZ deployment (synchronous standby)
C) Manual daily snapshots
D) DynamoDB Global Tables

<details><summary>Show Answer</summary>

**Answer: B**

RDS Multi-AZ uses synchronous replication to the standby, meaning a transaction isn't considered committed until it's been written to both the primary and standby — this guarantees zero data loss on failover, unlike asynchronous read replicas (A) where some recent transactions could be lost if the primary fails before replication catches up.
</details>

---

### Question 22
A company wants to ensure that even if their primary AWS Region experiences a complete outage, their static marketing website (hosted on S3 + CloudFront) remains available to users. What approach best achieves this with minimal added complexity?

A) Accept the risk since the website is non-critical
B) Replicate the S3 bucket to a second Region via Cross-Region Replication, and use Route 53 failover routing between the two origins/CloudFront distributions
C) Move the entire website to a single EC2 instance instead
D) Use only one Region but increase the EC2 instance size

<details><summary>Show Answer</summary>

**Answer: B**

Cross-Region Replication keeps a synchronized copy of the static content in a second Region, and Route 53 failover routing (with health checks) automatically redirects users to the secondary origin if the primary becomes unreachable — a low-complexity, serverless DR pattern well suited to static content.
</details>

---

### Question 23
A company's order-processing Lambda function occasionally fails due to a downstream dependency timing out, and they want failed asynchronous invocations to be automatically captured for later analysis rather than silently discarded after retries are exhausted. What should be configured?

A) Increase the Lambda timeout to infinity
B) Configure a Dead-Letter Queue or Lambda Destinations (on-failure) pointing to an SQS queue or SNS topic
C) Disable retries entirely
D) Run the Lambda function synchronously only

<details><summary>Show Answer</summary>

**Answer: B**

For asynchronous Lambda invocations, configuring a DLQ (or the newer Lambda Destinations "on failure" option) captures the event payload after retries are exhausted, sending it to SQS or SNS for investigation instead of silently dropping it.
</details>

---

### Question 24
A company wants to validate that their disaster recovery plan actually works before a real disaster occurs, without impacting their production environment. What should they regularly do?

A) Never test DR since it's risky
B) Periodically perform DR drills/game days, failing over to the secondary site/region in a controlled manner and validating recovery time and data integrity
C) Only read the DR documentation
D) Assume backups are valid without ever restoring them

<details><summary>Show Answer</summary>

**Answer: B**

A DR plan is only as good as its last successful test — regular game days/drills that actually exercise failover (and restoring from backups) are the only reliable way to confirm RTO/RPO targets can really be met and that the plan/documentation is accurate.
</details>

---

### Question 25
A company has a fleet of EC2 instances running a stateless web application. They want to ensure that if AWS needs to reclaim capacity from Spot Instances, the application continues running without interruption. What architecture should they use?

A) Run the entire fleet on Spot Instances only, with no On-Demand instances
B) Use a mixed instance policy in the Auto Scaling group combining On-Demand and Spot Instances, ensuring a baseline of On-Demand capacity for availability
C) Avoid Auto Scaling groups entirely
D) Use Spot Instances only for the database tier

<details><summary>Show Answer</summary>

**Answer: B**

A mixed instances policy lets an ASG maintain a guaranteed baseline of On-Demand capacity (for availability) while using Spot Instances for additional cost-effective capacity; if Spot capacity is reclaimed, the ASG launches replacements (Spot or On-Demand per policy) to maintain desired capacity, balancing cost and resilience. Running entirely on Spot (A) risks interruption of the whole fleet at once if capacity is reclaimed.
</details>

---

### Question 26
A company runs a critical workload and wants to be alerted automatically — before customers notice — if application latency exceeds a threshold or error rates spike, so the operations team can respond proactively. What should be configured?

A) Manually checking logs once a day
B) Amazon CloudWatch Alarms on relevant metrics (e.g., latency, 5xx error rate), with notifications via Amazon SNS
C) Relying solely on customer support tickets
D) Disabling monitoring to reduce cost

<details><summary>Show Answer</summary>

**Answer: B**

CloudWatch Alarms continuously evaluate metrics against defined thresholds and can trigger SNS notifications (email, SMS, paging systems, or automated remediation via Lambda) the moment a threshold is breached, enabling proactive rather than reactive incident response.
</details>

---

[← Back to README](./README.md)
