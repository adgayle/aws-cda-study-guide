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