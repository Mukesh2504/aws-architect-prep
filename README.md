
# AWS Cloud & Data Analytics: Practical SAA Notes and Architecture Guide

> ✍️ **Prepared & Authored by:** [RAGULRAAJAN](https://github.com/RAGULRAAJAN)

A practical collection of AWS concepts covering compute, storage, networking, IAM, organizations, databases, data lakes, analytics, governance, and modern data architecture.

<details open>
<summary><strong>📚 Quick Navigation / Table of Contents</strong> (60 Study Topics & Architecture Patterns)</summary>

- [1. AWS Global Infrastructure](#1-aws-global-infrastructure)
- [2. Amazon EC2](#2-amazon-ec2)
- [3. Amazon Machine Image (AMI)](#3-amazon-machine-image-ami)
- [4. EC2 vs Serverless](#4-ec2-vs-serverless)
- [5. AWS Lambda](#5-aws-lambda)
- [6. Containers on AWS](#6-containers-on-aws)
- [7. VPC and CIDR](#7-vpc-and-cidr)
- [8. Security Groups vs Network ACLs](#8-security-groups-vs-network-acls)
- [9. Route Tables](#9-route-tables)
- [10. AWS Storage](#10-aws-storage)
- [11. EC2 Instance Store](#11-ec2-instance-store)
- [12. Amazon EBS](#12-amazon-ebs)
- [13. EBS Snapshots](#13-ebs-snapshots)
- [14. Amazon S3](#14-amazon-s3)
- [15. S3 Storage Classes](#15-s3-storage-classes)
- [16. S3 Cross-Region Replication](#16-s3-cross-region-replication)
- [17. Amazon EFS and FSx](#17-amazon-efs-and-fsx)
- [18. AWS Storage Gateway](#18-aws-storage-gateway)
- [19. Databases on AWS](#19-databases-on-aws)
- [20. Amazon DynamoDB](#20-amazon-dynamodb)
- [21. Amazon Neptune](#21-amazon-neptune)
- [22. RDS Proxy](#22-rds-proxy)
- [23. SNS vs SQS](#23-sns-vs-sqs)
- [24. Amazon CloudFront](#24-amazon-cloudfront)
- [25. AWS Analytics Architecture](#25-aws-analytics-architecture)
- [26. Data Lake](#26-data-lake)
- [27. Data Warehouse](#27-data-warehouse)
- [28. Amazon EMR](#28-amazon-emr)
- [29. AWS Glue](#29-aws-glue)
- [30. Data Formats for Analytics](#30-data-formats-for-analytics)
- [31. Amazon Athena](#31-amazon-athena)
- [32. Amazon QuickSight](#32-amazon-quicksight)
- [33. Amazon OpenSearch](#33-amazon-opensearch)
- [34. Kinesis](#34-kinesis)
- [35. Data Ingestion Services](#35-data-ingestion-services)
- [36. AWS Lake Formation](#36-aws-lake-formation)
- [37. Lake Formation Permissions](#37-lake-formation-permissions)
- [38. Lake Formation Setup](#38-lake-formation-setup)
- [39. Lake Formation Personas](#39-lake-formation-personas)
- [40. Lake Formation Blueprints](#40-lake-formation-blueprints)
- [41. Modern Data Architecture](#41-modern-data-architecture)
- [42. Modern Data Movement Patterns](#42-modern-data-movement-patterns)
- [43. AWS Organizations](#43-aws-organizations)
- [44. Organizational Units (OUs)](#44-organizational-units-ous)
- [45. IAM](#45-iam)
- [46. IAM Groups vs OUs](#46-iam-groups-vs-ous)
- [47. IAM Identity Center](#47-iam-identity-center)
- [48. Service Control Policies (SCPs)](#48-service-control-policies-scps)
- [49. IAM vs SCP](#49-iam-vs-scp)
- [50. AWS Control Tower](#50-aws-control-tower)
- [51. AWS CloudFormation](#51-aws-cloudformation)
- [52. AWS Service Catalog](#52-aws-service-catalog)
- [53. Monitoring and Scaling](#53-monitoring-and-scaling)
- [54. Elastic Load Balancing](#54-elastic-load-balancing)
- [55. Vertical vs Horizontal Scaling](#55-vertical-vs-horizontal-scaling)
- [56. Amazon CloudWatch](#56-amazon-cloudwatch)
- [57. VPC Flow Logs](#57-vpc-flow-logs)
- [58. AWS Backup](#58-aws-backup)
- [59. AWS Systems Manager](#59-aws-systems-manager)
- [60. Quick Service Selection Guide](#60-quick-service-selection-guide)
- [Final Mental Model](#final-mental-model)
- [Core Architecture Patterns](#core-architecture-patterns)

</details>

---

## 1. AWS Global Infrastructure

### AWS Region
An AWS Region is a geographical area containing multiple Availability Zones.

### Availability Zone (AZ)
An Availability Zone is one or more isolated data centers within an AWS Region.

```text
AWS Region
├── Availability Zone A
│   ├── Data Center
│   └── Data Center
├── Availability Zone B
│   ├── Data Center
└── Availability Zone C
    └── Data Center

```

Using multiple AZs improves availability and fault tolerance.

### Choosing an AWS Region

Four major factors:

* Latency
* Service availability
* Cost
* Compliance

---

## 2. Amazon EC2

Amazon EC2 provides resizable virtual machines in AWS.

It is useful when you need:

* Full control over the operating system
* Custom software
* Long-running applications
* Applications that cannot easily be refactored for serverless

### EC2 Instance Types

An instance type indicates the instance family and size.

```text
c5.4xlarge
│ │
│ └── Instance size
└──── Instance family/generation

```

**Common families:**

* General purpose
* Compute optimized
* Memory optimized
* Storage optimized
* Accelerated computing
<img width="1024" height="536" alt="image" src="https://github.com/user-attachments/assets/09b2a072-8e1b-4590-a9d5-4744e3248aee" />

---

## 3. Amazon Machine Image (AMI)

An AMI is a template used to launch EC2 instances.

It contains information such as:

* Operating system
* Software
* Configuration
* Root volume information

```text
AMI
 ↓
Launch
 ↓
EC2 Instance

```

> **Note:** Think of an AMI as a blueprint for an EC2 server.

---

## 4. EC2 vs Serverless

### EC2

You manage the virtual machine.

```text
Application
    ↓
EC2
    ↓
OS
    ↓
Virtual infrastructure

```

You are responsible for more infrastructure management.

### Serverless

AWS manages the underlying servers.

**Examples:**

* AWS Lambda
* AWS Fargate

```text
Code / Container
       ↓
AWS
       ↓
Infrastructure managed by AWS

```

### Choosing between them

* Choose **EC2** when you need greater OS/infrastructure control.
* Choose **serverless** when you want to reduce infrastructure management and automatically scale based on demand.

---

## 5. AWS Lambda

AWS Lambda runs code without requiring you to provision or manage servers.

**Lambda handles:**

* Server management
* Capacity provisioning
* Automatic scaling
* OS maintenance
* Infrastructure administration

```text
Event
 ↓
Lambda
 ↓
Code execution

```

**Examples of events:**

* S3 object upload
* API Gateway request
* EventBridge event
* Scheduled event

> **Takeaway:** Lambda is powerful, but it is not automatically the best solution for every workload.

---

## 6. Containers on AWS

### Amazon ECR

Amazon Elastic Container Registry (ECR) stores container images.

```text
Docker Image
     ↓
Amazon ECR

```

### Amazon ECS

Amazon Elastic Container Service (ECS) is AWS's managed container orchestration service.

### Amazon EKS

Amazon Elastic Kubernetes Service (EKS) is AWS's managed Kubernetes service.

* Choose **EKS** when you specifically need Kubernetes capabilities or compatibility.
* Choose **ECS** when you want simpler AWS-native container orchestration without Kubernetes complexity.

### AWS Fargate

Fargate is a serverless compute engine for containers. It can be used with:

* ECS
* EKS

With Fargate, AWS manages the underlying compute infrastructure.

> **Note:** Fargate does not provide normal SSH access to the underlying host because AWS manages that infrastructure.

---

## 7. VPC and CIDR

An Amazon VPC is your logically isolated virtual network in AWS. When creating a VPC, you specify a CIDR block.

**Example:**
`10.0.0.0/16`

CIDR defines the IP address range available for resources inside the VPC.

**A VPC can contain:**

* Subnets
* Route tables
* Internet Gateway
* NAT Gateway
* Security Groups
* Network ACLs

---

## 8. Security Groups vs Network ACLs

### Security Group

* Attached to resources such as EC2
* Stateful
* No inbound traffic allowed by default
* All outbound traffic allowed by default

### Network ACL

* Associated with subnets
* Stateless
* Default NACL allows inbound and outbound traffic
* Supports explicit allow and deny rules

> 💡 **Memory Check:**
> * **Security Group** → Resource level → Stateful
> * **NACL** → Subnet level → Stateless

---

## 9. Route Tables

A route table determines where network traffic should go.

**For a public subnet:**

```text
Subnet
 ↓
Route Table
 ↓
Internet Gateway
 ↓
Internet

```

A public subnet generally requires a route such as:
`0.0.0.0/0 → Internet Gateway`

---

## 10. AWS Storage

AWS provides several storage models:

| Storage | AWS Service | Typical Use |
| --- | --- | --- |
| **Object** | S3 | Files, media, logs, backups |
| **Block** | EBS | EC2 disks, databases |
| **File** | EFS / FSx | Shared file systems |
| **Temporary block** | EC2 Instance Store | Cache, scratch data |

---

## 11. EC2 Instance Store

Instance Store provides temporary block storage physically attached to the EC2 host. It is ephemeral.

```text
EC2
 ↓
Instance Store

```

If the instance is terminated, the instance-store data is lost.

**Good for:**

* Caches
* Buffers
* Temporary data
* Scratch space
* Distributed workloads where data is replicated elsewhere

---

## 12. Amazon EBS

Amazon Elastic Block Store (EBS) provides persistent block storage for EC2.

```text
EC2
 ↓
EBS Volume

```

**EBS is useful for:**

* EC2 root volumes
* Databases
* Transactional workloads
* Persistent application data

Unlike instance store, EBS data persists independently of the lifecycle of the EC2 instance, subject to volume configuration and deletion behavior.

### EBS Volume Categories

* **SSD**
* General Purpose SSD
* Provisioned IOPS SSD
* *Best for random I/O and latency-sensitive workloads.*

* **HDD**
* Throughput Optimized HDD
* Cold HDD
* *Best for sequential, throughput-oriented workloads.*

---

## 13. EBS Snapshots

EBS snapshots are backups of EBS volumes. They are incremental after the first snapshot.

```text
EBS Volume
    ↓
Snapshot
    ↓
Backup

```

Snapshots can be used to create new EBS volumes.

---

## 14. Amazon S3

Amazon S3 is object storage.

**It is commonly used for:**

* Images
* Videos
* Logs
* Backups
* Data lakes
* Static websites

S3 is designed for extremely high durability.

**S3 is not:**

* EC2 block storage
* A normal file system
* A relational database

---

## 15. S3 Storage Classes

**Important examples:**

* **S3 Standard** → Frequently accessed data
* **S3 Standard-IA** → Infrequently accessed data
* **S3 Intelligent-Tiering** → Automatically optimizes storage tier based on access patterns
* **S3 Glacier** → Archive storage
* **S3 Glacier Deep Archive** → Very rarely accessed long-term archive data

> **Example:** Seven years of rarely accessed data $\rightarrow$ **S3 Glacier Deep Archive**

---

## 16. S3 Cross-Region Replication

S3 Cross-Region Replication (CRR) automatically replicates objects from one S3 bucket to another bucket in a different AWS Region.

```text
Mumbai S3
    │
    │ CRR
    ▼
Singapore S3

```

**Uses include:**

* Disaster recovery
* Geographic redundancy
* Compliance
* Lower-latency access for users in another Region

Source and destination buckets can belong to different AWS accounts. Encrypted objects can also be replicated with appropriate configuration and permissions.

---

## 17. Amazon EFS and FSx

### Amazon EFS

Elastic File System provides a managed, scalable NFS file system. Multiple resources can access the same file system.

```text
EC2 ─┐
EC2 ─┼── EFS
EC2 ─┘

```

### Amazon FSx

Amazon FSx provides managed file systems for specific workloads. Examples include file systems based on:

* Windows File Server
* Lustre
* NetApp ONTAP
* OpenZFS

> 💡 **Think:**
> * **EFS** = scalable Linux/NFS file system
> * **FSx** = managed specialized file systems

---

## 18. AWS Storage Gateway

AWS Storage Gateway connects on-premises environments with AWS cloud storage. It supports hybrid storage use cases.

**Important types include:**

* S3 File Gateway
* FSx File Gateway
* Volume Gateway
* Tape Gateway

```text
On-Premises Application
        ↓
Storage Gateway
        ↓
AWS Storage

```

Useful when an organization wants cloud storage while maintaining on-premises applications.

---

## 19. Databases on AWS

### Amazon RDS

Amazon RDS is a managed relational database service. It supports relational database engines and provides managed capabilities such as:

* Backups
* Patching
* Monitoring
* Multi-AZ deployments

You still manage database-level activities such as schema and query optimization.

* **Multi-AZ:** Multi-AZ improves database availability and fault tolerance.
* **Read Replicas:** Read replicas are primarily used to scale read workloads.

```text
Application
   │
   ├── Primary DB
   │
   ├── Read Replica
   └── Read Replica

```

---

## 20. Amazon DynamoDB

DynamoDB is a managed NoSQL database. It uses:

* Tables
* Items
* Attributes

**A useful comparison:**

* **Relational DB:** `Table → Row → Column`
* **DynamoDB:** `Table → Item → Attribute`

**DynamoDB is suitable for:**

* Key-value workloads
* High-scale applications
* Low-latency access
* Applications requiring flexible schemas

---

## 21. Amazon Neptune

Amazon Neptune is a managed graph database.

**Suitable for:**

* Social networks
* Recommendation engines
* Knowledge graphs
* Relationship-heavy data

---

## 22. RDS Proxy

RDS Proxy manages database connections between applications and RDS using connection pooling.

Instead of every application request creating a new database connection:

```text
Applications
   ↓
RDS Proxy
   ↓
Connection Pool
   ↓
RDS

```

The proxy reuses existing database connections.

> **Key Concept:** Connection pooling is about reusing database connections, not caching query results.

---

## 23. SNS vs SQS

### Amazon SNS

SNS is a pub/sub messaging service. One message can be delivered to multiple subscribers.

```text
Publisher
    ↓
   SNS
 ┌──┼──┐
 ↓  ↓  ↓
SQS SQS Lambda

```

*Use SNS when you need fan-out.*

### Amazon SQS

SQS is a managed message queue.

```text
Producer
   ↓
 SQS
   ↓
Consumer

```

It decouples applications and lets consumers process messages asynchronously.

---

## 24. Amazon CloudFront

CloudFront is AWS's Content Delivery Network (CDN). It caches content at edge locations closer to users.

```text
User
 ↓
CloudFront Edge
 ↓
Origin

```

**Origins can include:**

* S3
* ALB
* EC2
* API Gateway
* Custom HTTP servers

**Benefits:**

* Lower latency
* Caching
* Reduced origin load
* HTTPS
* Integration with AWS WAF and Shield

**A common architecture is:**

```text
Users
  ↓
CloudFront
  ↓
S3

```

---

## 25. AWS Analytics Architecture

A typical modern analytics pipeline looks like:

```text
Data Sources
     ↓
Data Ingestion
     ↓
Amazon S3 Data Lake
     ↓
Glue / EMR
     ↓
Athena / Redshift / OpenSearch
     ↓
QuickSight

```

---

## 26. Data Lake

A Data Lake is a centralized repository capable of storing structured, semi-structured, and unstructured data from many sources.

Amazon S3 is the most common AWS foundation for a data lake.

**Examples of supported formats in S3:**

* CSV
* JSON
* Logs
* Images
* Videos
* Parquet
* XML

> 💡 **Memory Check:** Data Lake = Store raw data at scale.

---

## 27. Data Warehouse

A Data Warehouse stores structured, processed, and optimized data for analytics and business intelligence. Amazon Redshift is AWS's major data warehouse service.

### Data Lake vs Data Warehouse

| Feature | Data Lake | Data Warehouse |
| --- | --- | --- |
| **Data State** | Raw data | Processed data |
| **Structure** | Structured + unstructured | Mainly structured |
| **Schema** | Flexible schema | Schema-oriented |
| **Primary AWS Service** | S3 | Redshift |
| **Primary Use Case** | Data science / broad analytics | BI / reporting |

---

## 28. Amazon EMR

Amazon EMR is a managed big-data platform supporting frameworks such as:

* Hadoop
* Spark
* Hive
* Trino/Presto
* HBase

Use EMR when you need to process massive datasets.

```text
S3
 ↓
EMR
 ↓
Spark / Hadoop
 ↓
Processed Data

```

**Benefits:**

* Managed big-data infrastructure
* Scalable clusters
* Integration with S3
* Supports Hadoop/Spark workloads

---

## 29. AWS Glue

AWS Glue is a managed data integration and ETL service.

### Important Components:

* **Glue Crawler:** Discovers data structure/schema.
```text
Data
 ↓
Crawler
 ↓
Schema
 ↓
Glue Data Catalog

```

* **Glue Data Catalog:** Stores metadata about datasets (Tables, Columns, Schema, Partitions, Data locations).
* **Glue DataBrew:** A no-code tool for cleaning and preparing data.
* **Glue Studio:** A visual interface for designing ETL jobs.
* **Glue Workflows:** Coordinate multiple Glue jobs, crawlers, and triggers as a single workflow.

---

## 30. Data Formats for Analytics

Two important columnar formats are **Apache Parquet** and **Apache ORC**.

**They can improve:**

* Query performance
* Compression
* Storage efficiency
* Query cost

Columnar formats allow analytics engines to read only the columns they need:

* `CSV → larger scans`
* `Parquet/ORC → efficient column scans`

---

## 31. Amazon Athena

Athena is a serverless SQL query service. It can query data directly in S3 without provisioning database servers.

```text
S3
 ↓
Athena
 ↓
SQL Query

```

### Athena Federated Query

Allows queries across multiple data sources directly:

```text
One SQL Query
      ↓
S3 / RDS / DynamoDB

```

*(Without first moving all data into one location)*

---

## 32. Amazon QuickSight

QuickSight is a serverless business intelligence and visualization service used to create dashboards, charts, reports, and visual analytics.

```text
S3 / Athena / Redshift / RDS
             ↓
        QuickSight
             ↓
        Dashboard

```

---

## 33. Amazon OpenSearch

**OpenSearch is useful for:**

* Search
* Log analytics
* Operational analytics
* Text-based analysis

> **Note:** It is not a replacement for every database. Use it when fast search and indexing are the primary requirements.

---

## 34. Kinesis

Amazon Kinesis is used for real-time data streaming and ingestion.

```text
IoT Devices
    ↓
Kinesis
    ↓
Real-time processing

```

* **Kinesis Data Streams:** Designed for real-time streaming with low latency and applications that need to process streams directly.
* **Kinesis Data Firehose:** Managed service for delivering streaming data to destinations such as S3, Redshift, and OpenSearch.

---

## 35. Data Ingestion Services

* **AWS DMS:** Migrates and continuously replicates database data.
```text
Source DB → DMS → Target DB / S3 / Redshift

```

* **AWS SCT:** Converts database schemas between different database engines.
> 💡 **Memory Check:** SCT = Structure | DMS = Data

* **AWS Transfer Family:** Provides managed file transfers using SFTP, FTPS, FTP, and AS2.
* **Amazon AppFlow:** Moves data between SaaS applications and AWS services.
```text
Salesforce → AppFlow → S3

```

* **AWS Snow Family:** Uses physical devices to transfer very large datasets when network transfer is impractical.
```text
On-Premises → Snowball → AWS

```

---

## 36. AWS Lake Formation

AWS Lake Formation helps build, secure, govern, and share data lakes. It works closely with Amazon S3, AWS Glue Data Catalog, Athena, EMR, Redshift, and QuickSight.

* **Main purpose:** Fine-grained data lake governance.
* **Access Control Levels:** Database level, Table level, Column level, Row level, Cell level.

---

## 37. Lake Formation Permissions

If a resource is managed by Lake Formation:

```text
User
 ↓
Athena / EMR / Glue
 ↓
Glue Data Catalog
 ↓
Lake Formation
 ↓
Permission check
 ↓
Authorized data

```

If the resource isn't managed by Lake Formation, normal IAM/S3 permissions can control access. Lake Formation therefore augments IAM, rather than simply replacing it.

---

## 38. Lake Formation Setup

A basic setup involves:

1. Register the storage location
2. Create a database
3. Grant permissions

> 💡 **Memory Check:** Register → Create → Grant

---

## 39. Lake Formation Personas

* **Data Lake Administrator:** Manages the lake and its permissions.
* **Other personas:** Data engineers, Data analysts, Business analysts.

---

## 40. Lake Formation Blueprints

Lake Formation Blueprints provide predefined templates for creating data ingestion workflows. They automate parts of data ingestion, Glue jobs, crawlers, and workflow scheduling.

> 💡 **Think:** Blueprint = Pre-built data ingestion workflow.

---

## 41. Modern Data Architecture

A modern AWS data architecture combines Data Lakes, Data Warehouses, purpose-built databases, unified access, and unified governance.

**The Five Pillars:**

1. Scalable Data Lakes
2. Purpose-Built Analytics Services
3. Unified Data Access
4. Unified Governance
5. Performant and Cost-Effective

---

## 42. Modern Data Movement Patterns

* **Inside-Out (Data Lake → Specialized data store):**
```text
S3 → OpenSearch / Neptune

```

*Example:* Move data from S3 into Neptune to build a knowledge graph.
* **Outside-In (Specialized data store → Data Lake):**
```text
DynamoDB → S3

```

*Example:* Export gaming data from DynamoDB into S3 for analytics.
* **Around the Perimeter (Specialized data store → Specialized data store):**
```text
RDS → DynamoDB

```

*The Data Lake does not necessarily participate.*

---

## 43. AWS Organizations

AWS Organizations centrally manages multiple AWS accounts.

```text
AWS Organization
       ↓
      OUs
       ↓
AWS Accounts

```

**Companies create separate accounts for:**

* Development
* Testing
* Production
* Security
* Different business units

**Multiple accounts provide:** Security isolation, separate billing, reduced blast radius, governance, and service quota distribution.

---

## 44. Organizational Units (OUs)

An OU is a logical grouping of AWS accounts.

```text
AWS Organization
       ↓
Production OU
   ├── Account A
   └── Account B

```

> **Note:** OUs do not contain IAM users.

---

## 45. IAM

IAM manages identities and permissions inside an AWS account. An account can contain IAM Users, IAM Groups, IAM Roles, and IAM Policies.

> **Important distinction:** IAM User = identity, not automatically an administrator. A user becomes an administrator only if appropriate administrator permissions are granted.

---

## 46. IAM Groups vs OUs

This distinction is fundamental:

```text
IAM Group                      OU
    ↓                           ↓
 Alice                       Account A
  Bob                        Account B
Charlie                      Account C

```

> 💡 **Memory Check:** IAM Group = People | OU = Accounts

---

## 47. IAM Identity Center

IAM Identity Center provides centralized Single Sign-On (SSO) access to multiple AWS accounts and applications.

```text
Employee
   ↓
IAM Identity Center
   ↓
Account 1 / Account 2 / Account 3

```

It can integrate with existing identity providers such as corporate directories.

> 💡 **Memory Check:** One login → many AWS accounts.

---

## 48. Service Control Policies (SCPs)

SCPs are governance guardrails used with AWS Organizations. They define the maximum available permissions for accounts or OUs.

```text
Production OU
      ↓
SCP (Deny EC2 termination)

```

> **Note:** Even if an IAM policy allows the action, an explicit SCP deny prevents it.

---

## 49. IAM vs SCP

* **IAM Policy answers:** *What can this identity do?*
* **SCP answers:** *What can identities in this account potentially do at maximum?*

Both layers matter.

---

## 50. AWS Control Tower

AWS Control Tower helps set up and govern a multi-account AWS environment by orchestrating:

* AWS Organizations
* IAM Identity Center
* AWS Service Catalog
* AWS CloudFormation

It provides governance controls/guardrails and helps establish a landing zone.

> 💡 **Memory Check:** Control Tower = Multi-account governance.

---

## 51. AWS CloudFormation

CloudFormation is an Infrastructure as Code (IaC) service. Instead of manually creating resources (VPC, EC2, RDS, Subnets, Load Balancer), you define them in a template.

```text
CloudFormation Template → AWS Resources

```

**Benefits:** Automation, repeatability, version control, and consistent infrastructure.

---

## 52. AWS Service Catalog

Service Catalog allows administrators to create a catalog of approved AWS products that users can deploy.

```text
Administrator → Approved Products → Service Catalog → Users

```

> 💡 **Memory Check:**
> * **CloudFormation** = Build
> * **Service Catalog** = Approved products
> * **Control Tower** = Govern the organization

---

## 53. Monitoring and Scaling

### EC2 Auto Scaling

Three important components:

```text
Launch Template → Auto Scaling Group → Scaling Policies

```

---

## 54. Elastic Load Balancing

ELB distributes incoming traffic across backend targets and integrates with EC2 Auto Scaling.

### Application Load Balancer

ALB operates at the application layer and supports path-based routing:

```text
/example  ──► Target Group A
/api      ──► Target Group B

```

---

## 55. Vertical vs Horizontal Scaling

* **Vertical Scaling:** Increase the size of a resource (`Small EC2 → Large EC2`).
* **Horizontal Scaling:** Increase the number of resources (`1 EC2 → 3 EC2`).

*Cloud-native architectures commonly favor horizontal scaling.*

---

## 56. Amazon CloudWatch

CloudWatch provides monitoring and observability by collecting Metrics, Logs, Events, and Alarms.

**Alarm states:**

* `OK`
* `ALARM`
* `INSUFFICIENT_DATA`

---

## 57. VPC Flow Logs

VPC Flow Logs capture information about network traffic flowing to and from network interfaces in your VPC.

**They can help with:**

* Network troubleshooting
* Security investigation
* Traffic analysis

---

## 58. AWS Backup

AWS Backup centrally manages backups across supported AWS services (EBS, EFS, RDS, DynamoDB, FSx).

```text
EBS / EFS / RDS / DynamoDB / FSx ──► AWS Backup

```

> 💡 **Memory Check:** AWS Backup = Centralized backup management

---

## 59. AWS Systems Manager

Systems Manager helps manage and operate resources.

**Capabilities:** Run Command, Patch Manager, Session Manager, Parameter Store, Automation.

* **Session Manager:** Allows secure access to managed instances without requiring normal SSH access or opening port 22.

> 💡 **Memory Check:** Systems Manager = Manage servers

---

## 60. Quick Service Selection Guide

| Requirement | AWS Service |
| --- | --- |
| Virtual machine | **EC2** |
| EC2 template | **AMI** |
| Serverless code | **Lambda** |
| Serverless containers | **Fargate** |
| Container orchestration | **ECS** |
| Kubernetes | **EKS** |
| Container image registry | **ECR** |
| Object storage | **S3** |
| EC2 block storage | **EBS** |
| Temporary EC2 storage | **Instance Store** |
| Shared NFS storage | **EFS** |
| Specialized file system | **FSx** |
| Managed relational DB | **RDS** |
| NoSQL key-value | **DynamoDB** |
| Graph database | **Neptune** |
| DB connection pooling | **RDS Proxy** |
| Messaging queue | **SQS** |
| Pub/sub fan-out | **SNS** |
| CDN | **CloudFront** |
| Real-time streaming | **Kinesis** |
| Database migration | **DMS** |
| Schema conversion | **SCT** |
| SaaS data integration | **AppFlow** |
| SFTP/FTP | **Transfer Family** |
| Massive physical migration | **Snow Family** |
| Big-data processing | **EMR** |
| ETL | **Glue** |
| Data catalog | **Glue Data Catalog** |
| Schema discovery | **Glue Crawler** |
| No-code data preparation | **Glue DataBrew** |
| Visual ETL | **Glue Studio** |
| SQL on S3 | **Athena** |
| Data warehouse | **Redshift** |
| Search/log analytics | **OpenSearch** |
| BI dashboards | **QuickSight** |
| Data lake governance | **Lake Formation** |
| Multi-account management | **Organizations** |
| Account grouping | **OUs** |
| User permissions | **IAM** |
| SSO across accounts | **IAM Identity Center** |
| Account governance guardrails | **SCP** |
| Multi-account landing zone | **Control Tower** |
| Infrastructure as Code | **CloudFormation** |
| Approved infrastructure catalog | **Service Catalog** |
| Centralized backups | **AWS Backup** |
| Server management | **Systems Manager** |
| Monitoring | **CloudWatch** |

---

## Final Mental Model

The most important architecture to remember:

```text
                         AWS Organization
                                │
                       ┌────────┴────────┐
                       │                 │
                     OUs              Accounts
                                         │
                                        IAM
                                         │
                                  AWS Workloads
                                         │
              ┌──────────────────────────┼──────────────────────┐
              │                          │                      │
            Compute                  Storage                Database
              │                          │                      │
       EC2 / ECS / EKS              S3 / EBS              RDS / DynamoDB
       Lambda / Fargate             EFS / FSx              Neptune
              │                          │
              └──────────────┬───────────┘
                             │
                        Data Analytics
                             │
                         Amazon S3
                        Data Lake
                             │
                  ┌──────────┼──────────┐
                  │          │          │
                 Glue       EMR      Athena
                  │          │          │
                  └──────────┼──────────┘
                             │
                     Redshift / OpenSearch
                             │
                         QuickSight
                             │
                         Dashboards

```

### The Most Important Exam Mental Models

* **EC2** → You manage the server.
* **Lambda/Fargate** → AWS manages the underlying infrastructure.
* **S3** → Object storage and common Data Lake foundation.
* **EBS** → Persistent block storage for EC2.
* **RDS** → Managed relational database.
* **DynamoDB** → Managed NoSQL database.
* **S3 + CloudFront** → Globally delivered static content.
* **Glue** → Catalog + ETL.
* **EMR** → Massive-data processing with Hadoop/Spark.
* **Athena** → Serverless SQL on S3.
* **Redshift** → Data warehouse.
* **QuickSight** → Visualization/BI.
* **Lake Formation** → Data lake governance and fine-grained permissions.
* **IAM** → Identities and permissions within an account.
* **IAM Identity Center** → SSO across accounts.
* **OU** → Groups AWS accounts.
* **SCP** → Organization-level permission guardrails.
* **Control Tower** → Multi-account governance.
* **CloudFormation** → Infrastructure as Code.
* **Service Catalog** → Approved infrastructure products.

---

## Core Architecture Patterns

### 1. Web Application Architecture

```text
              USERS
                │
                ▼
          Route 53 / DNS
                │
                ▼
           CloudFront
                │
        ┌───────┴────────┐
        ▼                ▼
       S3              ALB
   Static Frontend       │
                         ▼
                  EC2 / ECS / Lambda
                         │
                         ▼
                    RDS / DynamoDB

```

### 2. Data Analytics Platform

```text
          DATA ANALYTICS PLATFORM

Data Sources
     │
     ▼
Kinesis / DMS / DataSync / AppFlow
     │
     ▼
Amazon S3 (Data Lake)
     │
     ├──────────► Glue ──► Catalog
     │
     ├──────────► EMR
     │
     ├──────────► Athena
     │
     └──────────► Redshift
                    │
                    ▼
                QuickSight

       Lake Formation
              │
              ▼
      Governance + Security

```

---

> The real skill is not memorizing 50 AWS service definitions. It is being able to look at a requirement, identify what kind of problem it is, and select the service that fits. That is the part that actually survives an interview after the multiple-choice exam has mercifully stopped asking whether S3 is a database.

---

## 👨‍💻 Author & Credits

This comprehensive guide was prepared and authored by **[RAGULRAAJAN](https://github.com/RAGULRAAJAN)**.

