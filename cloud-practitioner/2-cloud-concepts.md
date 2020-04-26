
# Cloud Concepts

## 1. Account Setup
_How to setup an AWS account_

#### AWS Organisations

**How it works:**
- Global Service
- Can create accounts fresh in the organisation
- Can invite accounts (emails the root user)
- You can't invite another organisation/ root account

**The hierarchy:**
- Policies stand alone (like IAM policies)
- Accounts are categorised into organisational units
- Policies are attached to organisational units

// TODO: 👷‍♀ Can an organisation invite another organisation?

#### The 4 Different Support Plans

1. Basic ($0PM) — Repsonses for billing, not technical.
1. Developer ($29PM) — Contact for technical questions 12-
1. Business ($100PM) — 24/7 support by phone, AWS Trusted Advisor.
1. Enterprise ($15000PM) — Get a TAM (Technical Account Manager) with 15m response time.

#### Quick Starts & AWS Landing Zone

**Quick Start**
- Templates designed by an architect (provisioned through CloudFormation)

// TODO : 👷‍♀ Learn more about this "service"

**Landing Zone**
- Deploys 4 accounts at the same time (quick setup of AWS accounts)

#### IAM

- Global service (not regional)
- **Hierarchy:*- Users, Groups, Roles
- 3 (kinda) ways of accessing AWS: Programattic access (and SDK access), console access.
- MFA is useful for users
- Create one user per person, assign groups and roles where necessary
- Apply password policy (complexity, expiration, password reuse)
- Policies come as JSON documents.

// TODO : 👷‍♀ Draw up the IAM relations (Role, Policy, User)
// TODO : 👷‍♀ Are group permissions additive?
// TODO : 👷‍♀ How do principals work?

## 2. Compute Services, Websites & Scaling.
_The services to be used as compute._

#### Using IAM Roles (with EC2)

A safer way to give priveledges to EC2 instances, if you use the AWS CLI within an instance with a role, it will get the privledges from the attached role.

1. Choose the service that will use the role.
1. Attach a policy to the role.
1. Tag the role.
1. Attach the role to the EC2.
1. Don't need credentials on the EC2, you can just use the role.

#### How To Use An Elastic Load Balancer

Three types of ALB exists...

1. Application Load Balancer (Layer 7)
1. Network Load Balancer (High Performance)
1. Classic Load Balancer (Being Deprecated)

Configuring an ALB:

- You need to configure your ALB for which AZ's which it can use.
- You'll need a target group for the servers that will receive your traffic.
- Configure your polling thresholds (how often it polls the server)
- Can take ~5 minutes to setup an ALB

// TODO: 👷‍♀ Active/Active or Active/Passive with ALB?

#### EC2

- Vertical server in the Cloud
- Instance Types
    - T3, Lowest Cost, General Purpose
    - M5, General Purpose
    - X1, Large Memory
- Make a web server
    - `yum install httpd -yes`
    - `service httpd start`
    - `echo '<p> Hello </p>' > /var/www/html`

#### Security Groups / Firewalls

- To allow a single IP you need to add `/32` to your IP to restrict just that IP

#### Using EC2 as a Web Server

- Storage Volumes attached to EC2 instances
- SSD type
    - GP2 - for general purpose
    - IO1 - for provisioned IOps (DB servers)
- Magnetic
    - ST1 - Lowest cost HDD (data warehouse)
    - SC1 - Lowest cost (file servers)

// TODO: 👷‍♀ Research more about these EBS types

#### AutoScaling

Two things to autoscaling...

1. Launch Configuration (how you want to launch your EC2)
1. Autoscaling Group

// TODO: 👷‍♀ Experiment with autoscaling groups

#### Elastic Beanstalk

- Kinda like Heroku (using AWS underlying resources)
- Launch basic applications (PHP, Node.JS for instance)

## 3. Operations + Monitoring

_Things that affect operations and monitoring..._

#### CloudWatch

- CloudWatch is for monitoring performance (metrics, logs)
- Can install custom metrics on AWS

#### Systems Manager

- Used for configuration management
- Uses "Run Command" on difference EC2's

// TODO: 👷‍♀ What's the difference between systems manager and ansible etc?

#### CloudTrail

// TODO: 👷‍♀ What is the purpose of a CloudTrail? When would you use it?

#### Athena vs Macie

**Athena:**
- Serverless (so you pay per query)
- Works with data in S3
- No complex ETL

**Macie:**
- A sub-feature of S3 / CloudTrail
- Uses NLP to protect PII on S3

// TODO: 👷‍♀ Read more about Macie, and what it is

## 4. Other Fundamental Services

**What is S3?**
- Flat "Object Based" storage
- Not "Block Storage" (like where you can store an Operating System)
- A bucket is a "folder" in the crowd
- Key Value (key the name of the item) Value (the value of the actual file data)

**S3 Availability**
- Consistency in AWS
    - Read after write consistency for new objects (Immediate updates)
    - Eventually consistent for overwrite PUTs (Slow updates, basically)
- Availability
    - 99.9% availability (built to be 99.99%)
    - Guarenteed 99.99999999999% (11 nines)

**S3 Storage Classes**
- S3 Storage Classes
    - Standard
    - IA
    - One Zone IA
    - S3 Intelligent Tiering (move data to best cost tier)
    - Glacier (and Glacier Deep Archive, i.e 12 hour retrieval)
- Transfer Acceleration
    - Upload to a region nearest to the user
- Cross Region Replication
    - Replicated to a secondary bucket (for disaster recovery)

**S3 Permissions**
- An ACL is on a file level (fine grained) bucket policy is on a bucket level
- Used to make buckets public
- You can do this on an individual object level

// TODO: 👷‍♀ Understand more about how ACL's work on S3

#### Databases

**Availability For Databases**
- Multi AZ which allows failover to a new availability zone
- Read replicas are sent to the replica

**OLTP vs OLAP**
- **OLTP** is running a query (i.e grabbing a row)
- **OLAP** usually done in a data warehouse (not on primary)... on AWS you have redshift

**ElastiCache**
- Two types of elastic cache: Memcached and Redis

**Aurora vs DynamoDB vs Redshift**
- Aurora: Scalable, multi-az, used for joinable data
- Dynamo: Use when you don't have joins, don't store large data in DynamoDB
- Redshift: Don't use for OLTP
- CloudSearch & ElasticSearch:
- Neptune: Graph database
- Elasticache: Caching in elasticache, cloudfront for CDN caching

#### CloudFront

- Helps distribute files around the world
- Cache is controlled by the TTL
- **Edge** is where it's cached
- **Origin** is the originator of the files
- **Distribution** which is the collection of edge locations

## 5. Miscellaneous Notes / Services

#### AWS CLI
- Format of AWS command: `aws service-name command`
- Add credentials to use AWS CLI

#### Global Services

Which services are global?

1. IAM
1. Route53
1. Cloudfront
1. SNS + SES

Remember, S3 is not global.

#### Which AWS Services can be used on premise?

- **AWS Snowball** - To import data into AWS.
- **Snowball Edge** - Computer with storage (comes with Lambda installed).
- **Storage Gateway** - Replicates to S3.
- **Code Deploy** - Can deploy to on premise code.
- **OpsWorks** - Managed Chef, can be used to deploy to on-premise.
- **IoT Greengrass** - Managed Chef, can be used to deploy to on-premise.

// TODO: 👷‍♀

#### Linux

- `sudo su` —> Stands for switch user
- Fedora / RedHat / CentOS
    - Installs with: `apt-get`
- Ubuntu/Debian
    - Installs with: `yum`

#### DNS + S3 Websites

- Register a domain within the AWS console
- Creating a website bucket must be the same, i.e `yourwebsite.com`
