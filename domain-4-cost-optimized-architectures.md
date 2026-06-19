# Domain 4: Design Cost-Optimized Architectures (20 Questions)

---

### Question 1
A company has a steady-state production workload running 24/7 on EC2 for the next 3 years, with a known, stable instance family requirement. What purchasing option minimizes cost for this predictable workload?

A) On-Demand Instances
B) Spot Instances
C) A 3-year Reserved Instance (or Savings Plan) commitment
D) Dedicated Hosts billed hourly

<details><summary>Show Answer</summary>

**Answer: C**

For steady-state, predictable, long-running workloads, a longer-term commitment (Reserved Instances or Savings Plans) offers the deepest discount compared to On-Demand pricing, since the customer is trading flexibility for a significant price reduction by committing to usage. Spot (B) is unsuitable for workloads that can't tolerate interruption.
</details>

---

### Question 2
A company runs fault-tolerant, flexible batch-processing jobs that can be interrupted and resumed without issue. They want to minimize compute cost for this workload. What purchasing option is most cost-effective?

A) On-Demand Instances
B) Spot Instances
C) 1-year Reserved Instances
D) Dedicated Instances

<details><summary>Show Answer</summary>

**Answer: B**

Spot Instances offer the deepest discounts (often up to 90% off On-Demand) by using spare AWS capacity, in exchange for the possibility of interruption with short notice — an excellent fit for fault-tolerant, flexible workloads like batch processing that can checkpoint and resume.
</details>

---

### Question 3
A company has unpredictable compute needs across varying instance families and even across EC2, Fargate, and Lambda, but wants to still benefit from a usage-based discount instead of paying full On-Demand rates. What pricing model offers this flexibility?

A) Standard Reserved Instances (tied to a specific instance family/Region)
B) Compute Savings Plans
C) Spot Instances only
D) Dedicated Hosts

<details><summary>Show Answer</summary>

**Answer: B**

Compute Savings Plans apply a discount in exchange for a commitment to a consistent amount of compute usage (measured in $/hour), regardless of instance family, size, OS, Region, or even whether the usage is EC2, Fargate, or Lambda — offering much more flexibility than Standard RIs (A), which are locked to a specific instance family and Region.
</details>

---

### Question 4
A company stores infrequently accessed log files that must be retained for compliance but are rarely read, and when they are read, retrieval can take several hours. What S3 storage class minimizes storage cost for this use case?

A) S3 Standard
B) S3 Standard-IA
C) S3 Glacier Deep Archive
D) S3 Intelligent-Tiering

<details><summary>Show Answer</summary>

**Answer: C**

Glacier Deep Archive is the lowest-cost S3 storage class, designed for data that is rarely accessed and where retrieval times of up to 12 hours are acceptable — ideal for long-term compliance archives that are essentially never read. Standard-IA (B) is for data accessed less frequently but still needing millisecond retrieval, which costs more than Deep Archive.
</details>

---

### Question 5
A company has objects in S3 with unpredictable and changing access patterns — some become infrequently accessed and later become frequently accessed again — and they want storage costs automatically optimized without manually managing lifecycle transitions or risking retrieval fees for data that becomes active again. What should they use?

A) S3 Standard only
B) S3 Intelligent-Tiering
C) A manual lifecycle policy moving everything to Glacier after 30 days
D) S3 One Zone-IA for all objects

<details><summary>Show Answer</summary>

**Answer: B**

S3 Intelligent-Tiering automatically moves objects between access tiers based on observed access patterns, with no retrieval fees, making it ideal when access patterns are unpredictable or changing — manually moving everything to Glacier (C) would incur retrieval costs/delays if data becomes active again.
</details>

---

### Question 6
A company wants visibility into which specific teams, projects, or environments are driving AWS costs across a single consolidated bill spanning multiple AWS accounts. What should be implemented?

A) Nothing — costs can't be broken down within a consolidated bill
B) Cost allocation tags applied consistently to resources, combined with AWS Cost Explorer / Cost and Usage Reports
C) Switching every team to its own separate AWS Organization
D) Reserved Instances only

<details><summary>Show Answer</summary>

**Answer: B**

Applying consistent cost allocation tags (e.g., `team`, `project`, `environment`) to resources allows costs to be filtered and grouped by those dimensions in Cost Explorer and Cost and Usage Reports, giving granular visibility into spend even within a single consolidated bill across linked accounts.
</details>

---

### Question 7
A company wants a free, automated tool that analyzes their AWS account for cost optimization opportunities — such as idle/underutilized EC2 instances and unattached EBS volumes — alongside security and performance checks. What should they use?

A) AWS Trusted Advisor
B) Amazon Macie
C) AWS Config only
D) Amazon Inspector

<details><summary>Show Answer</summary>

**Answer: A**

Trusted Advisor inspects the account across cost optimization, performance, security, fault tolerance, and service limits categories, flagging things like idle load balancers, underutilized EC2 instances, and unattached EBS volumes — core checks are free for all accounts, with expanded checks on Business/Enterprise support plans.
</details>

---

### Question 8
A company has EC2 instances that are consistently running well below their provisioned CPU and memory capacity, indicating they're oversized for the actual workload. What service provides automated, data-driven recommendations on the right instance type/size to reduce cost?

A) AWS Compute Optimizer
B) AWS Shield
C) Amazon GuardDuty
D) AWS Direct Connect

<details><summary>Show Answer</summary>

**Answer: A**

Compute Optimizer analyzes historical utilization metrics (CPU, memory, network, etc.) and provides specific right-sizing recommendations for EC2 instances, Auto Scaling groups, EBS volumes, and Lambda functions, helping eliminate the cost of over-provisioned resources.
</details>

---

### Question 9
A development team's EC2 instances and RDS databases used for testing only need to run during business hours (8am–6pm, Monday–Friday) and are otherwise idle, wasting money overnight and on weekends. What is the most cost-effective way to address this?

A) Leave everything running 24/7 since stopping/starting is too complex
B) Use scheduled actions (e.g., via Instance Scheduler on AWS, or Auto Scaling scheduled actions / Lambda + EventBridge) to automatically stop resources outside business hours and start them again before work begins
C) Purchase Reserved Instances for these dev/test resources
D) Move the dev/test environment to a more expensive instance type

<details><summary>Show Answer</summary>

**Answer: B**

Scheduling automatic start/stop of non-production resources outside business hours (roughly 65 out of 168 hours/week are "business hours") can cut compute costs by well over half for dev/test workloads that don't need to run continuously. Reserved Instances (C) are intended for steady-state, always-on usage and don't fit an intermittent schedule well.
</details>

---

### Question 10
A company wants to avoid NAT Gateway data processing charges for traffic that EC2 instances in a private subnet send to Amazon S3, while keeping that traffic off the public internet. What should be configured?

A) Continue routing S3 traffic through the NAT Gateway
B) A Gateway VPC Endpoint for S3
C) A Direct Connect connection
D) An Internet Gateway attached to the private subnet

<details><summary>Show Answer</summary>

**Answer: B**

A Gateway VPC Endpoint for S3 routes S3 traffic privately within the AWS network at no additional data processing charge, avoiding both the NAT Gateway's per-GB data processing fee and the security exposure of routing through a NAT path unnecessarily.
</details>

---

### Question 11
A company's application has highly variable, often-idle database usage patterns — sometimes zero traffic for hours, then sudden bursts — making it hard to right-size a provisioned Aurora cluster cost-effectively. What Aurora feature automatically scales capacity (including down to near-zero) based on actual demand?

A) Aurora Provisioned with a fixed large instance size
B) Aurora Serverless v2
C) Amazon Redshift
D) RDS Multi-AZ with a static instance class

<details><summary>Show Answer</summary>

**Answer: B**

Aurora Serverless automatically scales database capacity up and down based on actual application demand, and can scale down significantly during idle periods, billing based on actual capacity consumed rather than a fixed provisioned size — well suited to unpredictable, intermittent workloads.
</details>

---

### Question 12
A company runs containerized batch jobs on ECS that are fault-tolerant and can be interrupted and retried, and wants to minimize compute cost for this workload while still using a managed container orchestration service. What should they use?

A) ECS tasks on the Fargate Spot capacity provider
B) ECS tasks on standard Fargate (On-Demand) only
C) EC2 Reserved Instances running ECS
D) Lambda with no container support

<details><summary>Show Answer</summary>

**Answer: A**

Fargate Spot applies Spot-style discounted pricing to Fargate tasks for fault-tolerant workloads that can tolerate interruption, combining the operational simplicity of serverless containers with substantial cost savings — directly matching a fault-tolerant batch job's requirements.
</details>

---

### Question 13
A company wants to set a hard ceiling alert so that finance is notified automatically via email when monthly AWS spend is forecasted to exceed a set budget, without needing to manually check the bill. What should be configured?

A) AWS Budgets with an alert threshold and notification action
B) AWS Trusted Advisor only
C) A manual monthly spreadsheet review
D) IAM policies restricting spend

<details><summary>Show Answer</summary>

**Answer: A**

AWS Budgets lets you set custom cost (or usage) budgets and configure alerts — including forecasted-spend alerts — that automatically notify specified recipients (e.g., via SNS/email) when actual or forecasted costs cross a defined threshold, without manual bill-checking.
</details>

---

### Question 14
A company has many small, short-lived EBS snapshots accumulating over time as part of their backup strategy, increasing storage costs unnecessarily. What should be implemented to automatically manage snapshot lifecycle and delete old, unneeded snapshots?

A) Manually delete snapshots once a year
B) Amazon Data Lifecycle Manager (DLM) or AWS Backup with a defined retention policy
C) Stop taking snapshots entirely
D) Move snapshots to S3 Glacier manually each time

<details><summary>Show Answer</summary>

**Answer: B**

Amazon Data Lifecycle Manager (or AWS Backup) automates the creation, retention, and deletion of EBS snapshots according to a defined schedule and retention policy, ensuring old snapshots are cleaned up automatically rather than accumulating indefinitely and driving up cost.
</details>

---

### Question 15
A company is comparing gp2 and gp3 EBS volumes for a workload that needs 6,000 IOPS and doesn't need IOPS to scale automatically with volume size. Which choice is more cost-effective?

A) gp2, since IOPS scale with volume size
B) gp3, since IOPS and throughput can be provisioned independently of size, often at lower cost
C) They are priced identically in all cases
D) io2 is the only option that supports 6,000 IOPS

<details><summary>Show Answer</summary>

**Answer: B**

gp3 decouples IOPS and throughput provisioning from volume size, letting you provision exactly the performance needed (e.g., 6,000 IOPS) without over-provisioning storage capacity just to get more IOPS as gp2 requires — this generally makes gp3 more cost-effective for performance needs that don't naturally scale with size.
</details>

---

### Question 16
A company transfers large amounts of data between EC2 instances in the same Availability Zone and is seeing higher-than-expected data transfer charges. What architectural change would reduce these costs?

A) Move instances to different Regions for redundancy
B) Ensure instances communicating frequently are placed within the same Availability Zone (and use private IP addresses) to take advantage of free/cheaper intra-AZ transfer where applicable
C) Route all traffic through a NAT Gateway
D) Use only public IP addresses for internal communication

<details><summary>Show Answer</summary>

**Answer: B**

Data transferred between instances in the same AZ using private IP addresses avoids the higher cross-AZ or internet-routed data transfer charges; routing internal traffic through a NAT Gateway (C) or over public IPs (D) when unnecessary increases cost without benefit.
</details>

---

### Question 17
A startup is unsure how much compute capacity they'll need over the next year and doesn't want to make any long-term financial commitment, prioritizing flexibility over discount depth. What pricing model should they default to?

A) 3-year All Upfront Reserved Instances
B) On-Demand Instances
C) 1-year No Upfront Savings Plans
D) Dedicated Hosts with annual billing

<details><summary>Show Answer</summary>

**Answer: B**

On-Demand pricing requires no upfront commitment and can be stopped/changed at any time, making it the right default when future usage is highly uncertain — committing to RIs or Savings Plans (A, C) before usage patterns are understood risks paying for unused commitment.
</details>

---

### Question 18
A company wants to reduce costs on a Lambda-based workload that has predictable, sustained execution time (e.g., a steady stream of invocations every day, every month, for the foreseeable future). What pricing option reduces Lambda cost for this predictable usage?

A) There is no discount mechanism for Lambda
B) A Compute Savings Plan, which also applies to Lambda usage
C) Reserved Instances applied directly to Lambda
D) Switching unconditionally to EC2 Spot Instances

<details><summary>Show Answer</summary>

**Answer: B**

Compute Savings Plans apply their committed-usage discount across EC2, Fargate, and Lambda, so a predictable, sustained Lambda workload can still benefit from a Savings Plan commitment even though traditional Reserved Instances (C) don't apply to Lambda at all.
</details>

---

### Question 19
A company stores S3 objects that are accessed frequently for the first 30 days after creation, rarely accessed for the next 60 days, and almost never accessed after 90 days but must be retained for 7 years for compliance. What is the most cost-effective storage configuration?

A) Keep everything in S3 Standard for the full 7 years
B) An S3 Lifecycle policy: S3 Standard for the first 30 days, transition to S3 Standard-IA at day 30, then transition to S3 Glacier Deep Archive at day 90, expiring at 7 years
C) Store everything in S3 Glacier Deep Archive from day one
D) Delete all objects after 90 days

<details><summary>Show Answer</summary>

**Answer: B**

This lifecycle policy matches storage class to actual access pattern at each stage — full-price Standard while access is frequent, cheaper Standard-IA during the moderate-access window, and the lowest-cost Glacier Deep Archive for the long compliance-retention tail — minimizing cost while meeting the 7-year retention requirement. Storing in Deep Archive from day one (C) would incur retrieval costs/delays during the initial high-access period.
</details>

---

### Question 20
A company's AWS Organization has 15 linked accounts. They want to take advantage of pooled Reserved Instance and Savings Plans discounts and volume pricing tiers across all accounts, rather than each account negotiating/purchasing separately. What enables this automatically?

A) Each account purchasing its own RIs independently with no sharing
B) AWS Organizations consolidated billing, which shares RI/Savings Plans discounts and volume pricing tiers across all member accounts by default
C) Manually invoicing each account separately every month
D) Disabling cost sharing entirely for security reasons

<details><summary>Show Answer</summary>

**Answer: B**

Consolidated billing under AWS Organizations combines usage across all linked accounts onto one bill and, by default, shares the benefit of Reserved Instance and Savings Plans discounts as well as volume-based pricing tiers across the whole organization — even if only one account purchased the RI/Savings Plan — maximizing the discount's value organization-wide.
</details>

---

[← Back to README](./README.md)
