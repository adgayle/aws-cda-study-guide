# AWS Certified Developer Architect - Associate

## Deployment
### Cloudformation
Cloudformation template sections
* AWSTemplateFormatVersion
* Description
* Metadata
* Parameters
* Mappings
* Conditions
* Resources (required)
* Outputs

Cloudformation functions
* Fn::Base64
* Fn::FindInMap
* Fn::Cidr
* Fn::GetAttr
* Fn::GetAZs
* Fn::ImportValue
* Fn::Join
* Fn::Select
* Fn::Select
* Fn::Split
* Fn::Sub
* Fn::Transform
* Fn::And
* Fn::Equals
* Fn::If
* Fn::Not
* Fn::Or
* Ref

### Serverless Services
* Lambda
* API Gateway
* S3
* DynamoDB
* SNS
* SQS
* Step Functions
* Kinesis
* Athena

### Lambda Languages
* Node.js
* Python
* Java
* C# 
* Go

### Lambda Triggers
* S3
* DynamoDB
* Kinesis
* Cognito
* Cloudformation
* Cloudtrail
* CodeCommit
* CloudWatch
* SES
* SQS
* SNS
* Cron
* API Gateway
* IoT
* Step Functions
* Alexa

### Deployment Phases
1. Source
1. Build
1. Test
1. Deploy
1. Monitor
Steps 1 & 2 are Continuous Integration
Steps 1, 2, 3 & 4 are Continuous Deployment
Steps 1, 2, 3, approval and 4 are Continuous Delivery 

### Highly Available and Scalable
1. Multiple Availability Zones and / or Regions
1. Health check and failover mechanisms
1. Stateless application. Keep session state on cache server (faster) or DB (slower)
1. Understand AWS services e.g. Autoscaling for HA and scalability

## Security

### Shared responsibility model
* Customer responsible for security in the cloud
* AWS responsible for security of the cloud

### S3 Encryption
* S3 Encryption at rest
    * S3 managed SSE-S3
    * KMS managed SSE-KMS
        * To rotate keys automatically KMS must generate the Customer Managed Key (CMK)
    * Customer managed SSE-C
    * Force server side encryption with request header x-amz-server-site-encryption
* S3 encryption in flight
    * Encryption in flight with TLS

### S3 Policy
* S3 has policy versions allowing easy backout

### IAM
* Roles
    * Role grants access without static credentials
    * Policies attached to role authorizes access
    * Roles give cross account access
* Global service
* Customizable sign in link
* Policies in JSON for user, group and role permissions
    * version
    * statements
    * effect
    * action
    * resource
    * principal
    * condition
* Role attached provides temporary access and secret keys
* Security Token Service (STS) 
    * API calls
        * AssumeRole
        * AssumeRolewithSAML
        * AssumeRoleWithWebIdentity
    * Temporary credentials 15min - 36hrs
    * Uses IAM policies
    * Supports SAML 2.0

* Federation
    * Web Identity
        * Cognito
            * Google
            * FaceBook
            * Amazon
            * OpenID
    * SAML
        * Active Directory
        * LDAPS
        * Open LDAP
* General Practices
    * Lockdown the root / master account
    * Security groups only allow traffic cannot deny
    * NACL allows and has explicit deny
    * Prefer IAM roles to access keys
* Cognito
    * Use for user pool from Google, FaceBook or Amazon
    * Use "Block Use" in advanced security for compromised credentials
    * Use for auto sign in & out processing

## Monitoring
AWS error codes
* 400 handle in the application (user errors)
* 500 use retries or exponential backoff (server errors)

Logs to collect and analyze
* System
* HTTP

Monitoring with CloudWatch
* Instance health
* Managed service health
* Network performance
* Utilization / cost efficiency
    * Under used or unused instances
* Use custom metrics

CloudTrail
* Who did what and when via the AWS API
* Does not track S3 or Lambda data events unless specifically enabled at additional cost

X-Ray
* Creates a service map of the application
* Identifies errors & bugs
* Identifies performance bottle necks
* Build your own analysis & visualization maps
* Use interceptors to capture all HTTP traffic 

DynamoDB ProvisionedThroughtputExceededException
* Exceeding capacity on partition key

Troubleshooting
* Check security groups & NACLs
* Private subnet EC2 cannot reach Internet without NAT
* Internet Gateway (IGW) & route needed to reach the Internet in public subnets
* EBS volumes except boot can be attached and detached from EC2 while running

## Refactoring
Best Practices
* Serverless provisioned at time of need
* Message queues handle communication between applications
* Static web assets are stored externally e.g. S3
* User authentication and user state storage are handled by managed AWS services
* Avoid any single points of failure
* Use caching e.g. Elasticache

Migration Strategies
* 6 R's: Rehost, Replatform, Repurchase, Refactor, Retain, Retire
* Durability & availability are different
* Scalability and elasticity are different
* Persistence and instance store do not go together
* Migrate monolith to microservices to functions
* Go serverless

## Development
### Best Practices
* Choose managed services
* Do not expose resources or APIs directly
* Use edge services & API gateway
* Do not store session state on server use Elasticache or DB
* Decouple your infrastructure e.g. use SQS

### S3
* Choose region based on
    * Latency
    * Cost
    * Regulatory requirements
* Each object has a unique key
* Path style URL (recommended)
* Virtual style URL (deprecated)
* Pre-signed URL use for sharing without authentication
* Cross origin resource sharing (CORS) to avoid cross site scripting errors
* Access
    * Bucket policy
    * Object level ACL

### DynamoDB
No SQL low latency database with tables, items (rows) and attributes (columns)
* Stores 3 copies across AZs
* Partitioned based on a partition key (hash)
* Sort key (composite key) aka. range key
* Primary key can be
    * partition key
    * partition key & sort key
* Fine grained access control using IAM
    * Use dynamoDB:LeadingKeys to access only items matching their UserID
* Secondary keys provide alternate search / query keys
    * 10GB max
* Secondary indexes
    * Enables fast queries on specific columns using alternative partition / sort keys
    * Local
        * Same partition key different sort key
        * Must be created at the same time as the table
    * Global
        * Partition and sort key are different from the table
        * Can be created at ant time
* Consistency model
    * Read capacity units (RCU)
        * Strongly consistent 1 x 4KB read per second
        * Eventually consistent 2 x 4KB read per second
    * Write capacity units (WCU)
        * 1 x 1KB write per second

DynamoDB Streams
* Captures new item
* Captures before and after for updates
* Captures before deletes
* Retained for 24 hours

DynamoDB Features
* Conditional write
* TTL to expire items
    * Use epoch number which is better than string dates for this purpose
* Optimistic locking
* Batch operations
    * BatchGetItem
    * BatchWriteItem

DynamoDB Accelerator (DAX)
* Caching for DynamoDB
    * Eventual consistency only
    * Helps read heavy workloads

Global Tables
* Use when replication to other regions is required

Performance Tips
* Reduce the impact of scans by reducing the page size
* Use ProjectExpressions parameter to restrict the attributes returned from a scan or query
* Query to find distinct item using primary key (cheap I/O) sorted ascending by default
* ScanIndexForward parameter reverses sort order for descending for query
* Avoid scans as they return the entire table (expensive I/O)
* Parallel scans are better than sequential scans by still have a high I/O impact
* Use Exponential Backoff aftr ProvisionedThroughputExceededException to wait longer between retries

### SNS
* Push messages
    * Topic
    * Subscriber
    * Message

### SQS
* Polled for messages
* Queue types
    * Standard (fastest)
    * FIFO (delivered at most once)
* Visibility timeout default 30 seconds (hides message during processing)
* Long polling (most cost efficient)
    * ReceiveMessageWaitTimeSeconds 20 seconds maximum setting

### Step Functions
* Coordinate components of distributed applications & microservices
* Visual workflows
* Built with Amazon state language

### API Gateway
* Multiple versions & stages
    * Use variables e.g. StageVariables.X where X is the name of the variable
* Import from formats
    * Swagger
    * OpenAPI

### CloudFront
* Path patterns
* Request headers, cookies, querystrings
* TTL (invalidations are billed used versions)
* Restrict access
    * Signed URL or Signed cookie

### Elasticache
* Use Redis for leaderboards, chat, queues & anything that needs to preserve the in memory cache (snapshots)
* Cannot change a subnet group after the cluster is deployed
* Memcache
    * Simple
    * No encryption
    * Multithreaded
    * Data partitions
    * No replication
    * No pub/sub
* Redis
    * Complex
    * Yes encryption
    * Not multithreaded
    * No data partitions
    * Yes replicated
    * Pub/sub
    * HIPAA, PCI and FedRamp compliant

### Kinesis
* Default retention is 24hrs maximum time is 7 days
* Maximum data blob size is 1MB
* Use shards to increase scale each supports 1000 put records per second
* ProvisionedThroughputExceeded fix by increasing shards (sustained)