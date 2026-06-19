# Domain 1: Design Secure Architectures (30 Questions)

---

### Question 1
A company stores sensitive financial records in an S3 bucket. Auditors require that all objects be encrypted at rest using keys that the company fully manages and can rotate, disable, or revoke at will, with a complete audit trail of every key usage. Which encryption option meets this requirement?

A) SSE-S3 (Amazon S3-managed keys)
B) SSE-KMS using an AWS managed key (`aws/s3`)
C) SSE-KMS using a customer managed key (CMK)
D) Client-side encryption with a hardcoded key stored in the application

<details><summary>Show Answer</summary>

**Answer: C**

A customer managed CMK in KMS gives the company full control over key policies, rotation, and the ability to disable/revoke the key, and every use of the key is logged to CloudTrail. SSE-S3 (A) and AWS managed keys (B) are controlled by AWS, not the customer, so the company can't manage rotation/revocation policy. D is operationally unsafe and not auditable.
</details>

---

### Question 2
A Solutions Architect needs to grant a Lambda function permission to read from a specific DynamoDB table, without storing any long-term credentials in the function's code or environment variables. What is the best approach?

A) Create an IAM user with DynamoDB access and store its access keys as Lambda environment variables
B) Attach an IAM execution role to the Lambda function with a policy scoped to that table
C) Embed the AWS account root credentials in the function
D) Use a NAT Gateway to proxy DynamoDB calls through a bastion host with stored credentials

<details><summary>Show Answer</summary>

**Answer: B**

Lambda functions assume an IAM execution role; AWS automatically rotates the temporary credentials behind the scenes, so no long-term secrets are stored anywhere. This follows the principle of least privilege and removes credential management entirely. A, C, and D all involve storing static/long-term credentials, which is an anti-pattern.
</details>

---

### Question 3
An application running on EC2 instances needs to call the S3 API. The Security team has mandated that no access keys be stored on any instance. What should the architect do?

A) Store access keys in a `.aws/credentials` file on each instance
B) Attach an IAM role to the EC2 instance profile
C) Pass access keys as user data at launch
D) Use the root account's access keys and rotate them monthly

<details><summary>Show Answer</summary>

**Answer: B**

An IAM role attached via an instance profile provides temporary, automatically rotated credentials through the instance metadata service — no static keys are ever stored on disk. The other options all involve embedding long-lived credentials, which violates the stated requirement.
</details>

---

### Question 4
A company wants developers to be able to launch EC2 instances only of type `t3.micro` or `t3.small`, and only within a specific VPC, regardless of which IAM permissions are otherwise attached to their roles. What should be used to enforce this organization-wide?

A) An IAM permissions boundary on each individual user
B) A Service Control Policy (SCP) in AWS Organizations
C) A resource-based policy on the EC2 service
D) A security group rule

<details><summary>Show Answer</summary>

**Answer: B**

SCPs set the maximum permissions for all accounts/users/roles within an AWS Organization (or organizational unit), acting as a guardrail that overrides even an `Allow` in an identity policy. Permissions boundaries (A) are scoped per-IAM-entity rather than organization-wide. Security groups (D) control network traffic, not API actions. EC2 has no relevant resource-based policy (C).
</details>

---

### Question 5
A web application's EC2 instances are in a private subnet behind an Application Load Balancer. Security wants to ensure only the ALB can initiate connections to the EC2 instances on port 443, and that this rule automatically stays correct even if the ALB's underlying IP addresses change. What should be configured?

A) A Network ACL rule allowing the ALB's current IP range
B) A security group rule on the EC2 instances that references the ALB's security group as the source
C) A NAT Gateway route directing all ALB traffic
D) A bastion host with an allow-list of ALB IPs

<details><summary>Show Answer</summary>

**Answer: B**

Security groups can reference another security group as the source, meaning any instance/resource (like an ALB) that is a member of that security group is automatically permitted — this stays correct even if underlying IPs change, since ALB IPs are dynamic. NACLs only support IP/CIDR-based rules and are stateless, making dynamic IP tracking impractical.
</details>

---

### Question 6
A company needs to store database credentials for an RDS instance and have them rotated automatically every 30 days without any application code changes beyond retrieving the current secret at runtime. Which service should be used?

A) AWS Systems Manager Parameter Store (Standard tier)
B) AWS Secrets Manager with automatic rotation enabled
C) Environment variables in the EC2 launch template
D) A hardcoded value in an S3 object with versioning enabled

<details><summary>Show Answer</summary>

**Answer: B**

Secrets Manager has native, built-in automatic rotation for supported RDS engines using a managed Lambda rotation function, requiring no custom rotation logic. Parameter Store (A) can store secrets but doesn't include native automatic rotation out of the box. C and D are static and not rotated.
</details>

---

### Question 7
A mobile application needs to allow users to sign in with Google or Facebook and then obtain temporary AWS credentials to upload photos directly to S3. Which AWS service is purpose-built for this?

A) AWS IAM Identity Center
B) Amazon Cognito (Identity Pools + User Pools)
C) AWS Directory Service
D) AWS STS AssumeRole with long-term IAM user keys

<details><summary>Show Answer</summary>

**Answer: B**

Cognito User Pools handle the social/federated sign-in, and Cognito Identity Pools then exchange that identity for short-lived, scoped AWS credentials via STS — exactly the mobile-app federated-access pattern. IAM Identity Center (A) is meant for workforce SSO into AWS accounts/apps, not consumer-facing mobile apps. D would require distributing long-term keys, which is insecure for a mobile client.
</details>

---

### Question 8
A company's security policy requires that any S3 bucket containing customer PII automatically have public access blocked at the bucket level, regardless of any bucket policy or ACL someone might later configure. What feature should be enabled?

A) S3 Object Lock
B) S3 Block Public Access
C) S3 Transfer Acceleration
D) S3 Requester Pays

<details><summary>Show Answer</summary>

**Answer: B**

S3 Block Public Access settings override any bucket policies or ACLs that would otherwise grant public access, providing a centralized safety control. Object Lock (A) is for write-once-read-many retention, not public access prevention.
</details>

---

### Question 9
An organization wants a continuous, automated check that flags any IAM user with access keys older than 90 days, and wants this check to run against all accounts in their AWS Organization. Which service is best suited?

A) Amazon Inspector
B) AWS Config with a managed rule, aggregated via a Config aggregator
C) Amazon Macie
D) AWS Trusted Advisor (Business support plan only, single account)

<details><summary>Show Answer</summary>

**Answer: B**

AWS Config continuously evaluates resource configurations against rules (including the managed `access-keys-rotated` rule), and a Config aggregator can collect compliance data across every account in an Organization into one view. Inspector (A) scans for software vulnerabilities on EC2/ECR/Lambda, not IAM hygiene. Macie (C) is for discovering sensitive data in S3.
</details>

---

### Question 10
A company runs a three-tier web application. The web tier is in a public subnet, and the application and database tiers are in private subnets. Security wants to explicitly deny traffic from a specific IP address that has been identified as malicious, in addition to the existing security group rules, at the subnet level. What should be used?

A) A new inbound security group rule with a Deny action
B) A Network ACL rule explicitly denying that IP address
C) AWS WAF on the web tier only
D) A route table entry pointing the IP to a black-hole route

<details><summary>Show Answer</summary>

**Answer: B**

Network ACLs are stateless and support explicit Allow and Deny rules evaluated in order, making them the right tool for blocking a specific malicious IP at the subnet boundary. Security groups (A) only support Allow rules — there's no explicit Deny. WAF (C) operates at the HTTP layer for the web tier only, not at the subnet/network level for all tiers.
</details>

---

### Question 11
A company wants to protect a public-facing web application from common web exploits like SQL injection and cross-site scripting, and also wants to rate-limit requests from any single IP address. Which service should be deployed in front of the application?

A) AWS Shield Standard
B) AWS WAF with rate-based rules
C) Security groups with stricter inbound rules
D) Amazon GuardDuty

<details><summary>Show Answer</summary>

**Answer: B**

AWS WAF operates at Layer 7 and includes managed rule groups for common exploits (SQLi, XSS) as well as rate-based rules to throttle abusive IPs. Shield Standard (A) protects against network/transport layer (L3/L4) DDoS only, automatically and free, but doesn't inspect HTTP content. GuardDuty (D) is a detection service, not an inline protection layer.
</details>

---

### Question 12
A Solutions Architect is designing cross-account access so that an application in Account A can read objects from an S3 bucket in Account B, without creating IAM users in Account B. What is the recommended approach?

A) Share the root account credentials of Account B
B) Create an IAM role in Account B with a trust policy allowing Account A to assume it, and a permissions policy scoped to the bucket
C) Make the S3 bucket fully public
D) Copy the IAM user's access keys from Account A into Account B

<details><summary>Show Answer</summary>

**Answer: B**

Cross-account IAM roles are the standard, secure mechanism: Account B defines a role with a trust policy naming Account A as a trusted principal, and Account A's application uses `sts:AssumeRole` to get temporary credentials scoped to exactly what's needed. This avoids creating or sharing any long-term credentials and avoids overly broad public access.
</details>

---

### Question 13
A healthcare company must ensure all data in transit between its application servers and clients is encrypted, and wants to enforce TLS termination with managed certificate renewal. Which combination best satisfies this with minimal operational overhead?

A) Self-signed certificates manually renewed every 90 days on each EC2 instance
B) AWS Certificate Manager (ACM) issuing a public certificate, attached to an Application Load Balancer listener
C) Disabling HTTPS and relying on a VPN tunnel for all client traffic
D) Storing a wildcard certificate in S3 and having the application download it at boot

<details><summary>Show Answer</summary>

**Answer: B**

ACM provisions and automatically renews public TLS certificates at no extra cost, and they attach directly to ALB/CloudFront listeners, with TLS termination handled by the managed service. A and D create manual renewal burden and risk of expiry; C does not satisfy "encrypted in transit" for typical web clients.
</details>

---

### Question 14
A company wants to detect potential account compromise, such as API calls from an unusual geographic location or communication with known malicious IP addresses, across all of its AWS accounts, using machine learning–based threat detection. Which service should be enabled?

A) AWS Config
B) Amazon GuardDuty
C) AWS CloudTrail alone
D) Amazon Inspector

<details><summary>Show Answer</summary>

**Answer: B**

GuardDuty continuously analyzes CloudTrail, VPC Flow Logs, and DNS logs using threat intelligence and machine learning to detect anomalies like unusual API call locations or known-bad IP communication. CloudTrail (C) only records API activity — it doesn't analyze it for threats on its own. Inspector (D) is for vulnerability scanning of workloads, not behavioral threat detection.
</details>

---

### Question 15
An organization stores large volumes of files in S3 and is concerned that some objects may inadvertently contain personally identifiable information (PII) like credit card numbers or social security numbers. Which service can automatically discover and classify this sensitive data?

A) Amazon Macie
B) AWS Config
C) Amazon Inspector
D) AWS Artifact

<details><summary>Show Answer</summary>

**Answer: A**

Macie uses machine learning and pattern matching specifically to discover, classify, and report on sensitive data (PII, financial data, credentials) stored in S3. AWS Artifact (D) is just a portal for compliance reports/agreements, not a scanning tool.
</details>

---

### Question 16
A company requires that all API calls made within its AWS account be logged immutably for at least seven years for compliance purposes, with protection against tampering or deletion even by account administrators. What should be configured?

A) CloudTrail logging to an S3 bucket with versioning only
B) CloudTrail logging to an S3 bucket with Object Lock in compliance mode and MFA delete enabled
C) CloudWatch Logs with a 7-day retention period
D) VPC Flow Logs sent to CloudWatch

<details><summary>Show Answer</summary>

**Answer: B**

S3 Object Lock in compliance mode prevents an object's retention settings from being overridden or the object deleted by any user, including the root user, until the retention period expires — combined with MFA delete this gives strong tamper protection for long-term, immutable CloudTrail logs. Versioning alone (A) doesn't prevent deletion of versions. VPC Flow Logs (D) capture network traffic metadata, not API calls.
</details>

---

### Question 17
A Solutions Architect needs to allow a third-party SaaS vendor's AWS account to access specific resources in the company's account, but only for the next 90 days, after which access should automatically and permanently end without manual cleanup. What is the most appropriate mechanism?

A) Create a permanent IAM user with an access key, and manually delete it after 90 days
B) Create a cross-account IAM role with a trust policy that includes a condition checking the current date, or use a time-bound external ID and review process
C) Share the root credentials temporarily
D) Add the vendor's IP range to the security group for 90 days and forget about it

<details><summary>Show Answer</summary>

**Answer: B**

Using a cross-account role avoids any long-term credential, and IAM trust/permission policies can include date-based or other conditions to constrain when access is valid; combined with a scheduled review/removal process this is the most secure approach to time-bound third-party access. A relies on manual cleanup and exposes long-term credentials. D only controls network reachability, not API-level authorization, and isn't a credential mechanism at all.
</details>

---

### Question 18
A company wants its internal employees, who already authenticate against an on-premises Microsoft Active Directory, to be able to single sign-on into the AWS Management Console without creating duplicate IAM users for each employee. What should be implemented?

A) Create an IAM user for every employee, synced manually
B) AWS IAM Identity Center (successor to AWS SSO) federated with the on-premises AD via AD Connector
C) Hardcode a shared IAM user/password for the whole team
D) Amazon Cognito User Pools

<details><summary>Show Answer</summary>

**Answer: B**

IAM Identity Center provides centralized SSO and can federate with an existing on-premises Active Directory (via AD Connector or AWS Managed Microsoft AD trust), letting employees use their existing corporate credentials to access AWS accounts without duplicate IAM users. Cognito (D) is intended for customer-facing application authentication, not workforce console access.
</details>

---

### Question 19
A company's compliance team requires that EBS volumes attached to EC2 instances are always encrypted, and wants to prevent any new unencrypted volume from ever being created in the account, even by mistake. What is the most effective control?

A) Manually checking each volume after creation
B) Enabling "EBS encryption by default" at the account/Region level
C) Adding a tag requirement on EBS volumes
D) Using S3 default encryption

<details><summary>Show Answer</summary>

**Answer: B**

Enabling EBS encryption by default at the account level (per Region) ensures that every newly created EBS volume and snapshot is automatically encrypted, regardless of how it's launched, with no possibility of an unencrypted volume slipping through. Tags (C) are metadata and don't enforce encryption. S3 default encryption (D) applies to S3 objects, not EBS.
</details>

---

### Question 20
A company wants to restrict access to an internal API hosted on API Gateway so that only requests originating from within their corporate VPC are accepted, with no public internet exposure at all. What API Gateway endpoint type should be used?

A) Edge-optimized endpoint
B) Regional endpoint with a public resource policy
C) Private endpoint using a VPC endpoint (interface endpoint) with a resource policy restricting to the VPC
D) A public endpoint protected only by an API key

<details><summary>Show Answer</summary>

**Answer: C**

Private API Gateway endpoints are only reachable through an interface VPC endpoint (powered by AWS PrivateLink) inside the specified VPC, and a resource policy can further restrict access to that VPC/VPC endpoint — this keeps the API entirely off the public internet. Edge-optimized (A) and regional (B/D) endpoints are reachable from the internet by default.
</details>

---

### Question 21
An application stores temporary session tokens in an Amazon S3 bucket. A Solutions Architect wants any object to be automatically and permanently deleted from the bucket exactly 24 hours after being written, with no possibility of recovery, and no application-side cleanup logic. What should be configured?

A) S3 Object Lock in governance mode
B) An S3 Lifecycle rule with an expiration action of 1 day
C) A daily Lambda function that scans and deletes old objects
D) S3 Intelligent-Tiering

<details><summary>Show Answer</summary>

**Answer: B**

An S3 Lifecycle expiration rule set to 1 day will automatically and permanently delete objects after that period, with no custom code required. Object Lock (A) is for *preventing* deletion, the opposite of what's needed here. A scheduled Lambda (C) works but adds unnecessary operational complexity compared to a native lifecycle rule.
</details>

---

### Question 22
A company wants developers to be able to create and manage their own S3 buckets, but absolutely never be able to delete the centrally managed CloudTrail logging bucket, even though their IAM policies otherwise grant `s3:*` permissions. What is the best way to enforce this?

A) Trust the developers not to delete it
B) Attach an explicit Deny statement (e.g., via an SCP or a resource-based bucket policy) that blocks `s3:DeleteBucket` on that specific bucket ARN for all principals except a designated admin role
C) Remove S3 console access entirely for all developers
D) Rename the bucket periodically so it's harder to find

<details><summary>Show Answer</summary>

**Answer: B**

An explicit Deny — whether in an SCP (organization-wide guardrail) or directly in the bucket's resource policy — always overrides any Allow granted elsewhere, including broad `s3:*` grants, providing a hard guarantee that the bucket can't be deleted except by the specified exception. Removing console access (C) doesn't stop API/CLI access.
</details>

---

### Question 23
A Solutions Architect wants to ensure that an IAM user can perform administrative actions only when they've authenticated using multi-factor authentication (MFA) in the current session. What should be added to the IAM policy?

A) A `Deny` statement with a condition checking `aws:MultiFactorAuthPresent` is `false`, for the relevant actions
B) Simply assigning the AdministratorAccess managed policy
C) A password policy requiring 14-character passwords
D) An S3 bucket policy requiring HTTPS

<details><summary>Show Answer</summary>

**Answer: A**

Adding a condition on `aws:MultiFactorAuthPresent` (denying access when it's false, or allowing only when true) enforces that administrative actions require an active MFA session, regardless of what permissions are otherwise granted. A strong password policy (C) addresses a different risk and doesn't enforce MFA per-session.
</details>

---

### Question 24
A company runs workloads across 20 AWS accounts and wants a single place to centrally manage and enforce preventive guardrails (like "never allow accounts to leave the Organization" or "never allow disabling of CloudTrail") across all accounts at once. What should be used?

A) Individually configure IAM policies in each of the 20 accounts
B) AWS Organizations with Service Control Policies applied at the OU or organization root level
C) AWS Config rules in each account
D) A shared EC2 bastion host

<details><summary>Show Answer</summary>

**Answer: B**

SCPs applied at the organization root or an OU automatically apply to every account underneath, providing centralized, preventive (not just detective) guardrails without needing to touch each account's IAM configuration individually. Config rules (C) are detective controls that report non-compliance but don't prevent the action.
</details>

---

### Question 25
A company wants to ensure that EC2 instances in a private subnet can download software updates from the internet (via package repositories) but cannot be initiated against from the internet, and wants to avoid any public IP addresses on those instances. What should be deployed?

A) An Internet Gateway attached directly to the private subnet's route table
B) A NAT Gateway in a public subnet, referenced in the private subnet's route table for outbound traffic
C) A bastion host with port forwarding
D) Assign Elastic IPs to each private instance

<details><summary>Show Answer</summary>

**Answer: B**

A NAT Gateway sits in a public subnet and allows instances in private subnets to initiate outbound connections to the internet (e.g., for updates) while remaining unreachable from inbound internet connections, and without needing public IPs on the instances themselves. An Internet Gateway directly on a private subnet's route table (A) would effectively make it public.
</details>

---

### Question 26
A Solutions Architect needs to give a contractor temporary access to perform a specific, limited task in the AWS account for one afternoon only, without creating a permanent IAM user. What is the best approach?

A) Create an IAM user, share the password, and remember to delete it later
B) Use AWS STS to generate temporary credentials via `AssumeRole`, with a short session duration and a tightly scoped policy
C) Share the architect's own personal credentials for the afternoon
D) Add the contractor to the Administrators group temporarily

<details><summary>Show Answer</summary>

**Answer: B**

STS `AssumeRole` issues short-lived, automatically expiring credentials scoped to exactly the permissions needed, eliminating both the long-term credential risk and the "remember to delete it later" failure mode. Sharing personal or admin credentials (C, D) violates least privilege and traceability.
</details>

---

### Question 27
A company's security team wants visibility into which IAM permissions are actually being used by a role versus which are merely granted, in order to tighten an overly permissive policy. Which tool helps identify unused permissions?

A) IAM Access Analyzer's policy generation / last accessed information
B) AWS Budgets
C) Amazon CloudWatch billing alarms
D) AWS Trusted Advisor cost checks

<details><summary>Show Answer</summary>

**Answer: A**

IAM Access Analyzer can generate a least-privilege policy based on actual CloudTrail activity, and IAM also surfaces "service last accessed" data, both of which help identify and remove permissions that are granted but never used. The other options relate to cost, not permissions auditing.
</details>

---

### Question 28
An application needs to make outbound calls to Amazon DynamoDB from EC2 instances in a private subnet, and the company wants this traffic to never traverse the public internet, even indirectly through a NAT Gateway. What should be configured?

A) A NAT Gateway with a restrictive route
B) A Gateway VPC Endpoint for DynamoDB
C) An Internet Gateway with a security group restricting to AWS IP ranges
D) AWS Direct Connect

<details><summary>Show Answer</summary>

**Answer: B**

A Gateway VPC Endpoint for DynamoDB (or S3) routes traffic to those services privately within the AWS network, without ever going over the public internet or through a NAT Gateway, and at no additional cost. Direct Connect (D) is for connecting on-premises networks to AWS, which isn't relevant here since both the EC2 instance and DynamoDB are already in AWS.
</details>

---

### Question 29
A company wants to ensure that even if an attacker obtains valid IAM credentials for a role, the resulting session cannot be used for longer than 15 minutes, limiting the blast radius of credential theft. What should be configured?

A) A strict password policy
B) The `MaxSessionDuration` setting on the IAM role, set to its minimum
C) Enabling GuardDuty
D) Enabling S3 versioning

<details><summary>Show Answer</summary>

**Answer: B**

`MaxSessionDuration` caps how long temporary credentials obtained by assuming a role remain valid, directly limiting how long stolen/leaked session credentials could be used. GuardDuty (C) can detect anomalous use after the fact but doesn't limit session lifetime.
</details>

---

### Question 30
A retail company processes credit card transactions and must comply with PCI DSS. They want documentation of AWS's own compliance certifications and audit reports to provide to their auditors. Where should they obtain this?

A) AWS Trusted Advisor
B) AWS Artifact
C) AWS Config conformance packs
D) Amazon Inspector reports

<details><summary>Show Answer</summary>

**Answer: B**

AWS Artifact is the self-service portal for downloading AWS's compliance reports (e.g., PCI DSS Attestation of Compliance, SOC reports) and agreements, which is exactly what's needed to provide to external auditors. Config conformance packs (C) help assess the *customer's* resource compliance, not AWS's own infrastructure compliance.
</details>

---

[← Back to README](./README.md)
