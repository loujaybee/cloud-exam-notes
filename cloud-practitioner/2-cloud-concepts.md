
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
- **Hierarchy:** Users, Groups, Roles
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

* You need to configure your ALB for which AZ's which it can use.
* You'll need a target group for the servers that will receive your traffic.
* Configure your polling thresholds (how often it polls the server)
* Can take ~5 minutes to setup an ALB

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

#### S3

- Highly durable object storage

## 3. Operations + Monitoring

_Things that affect operations and monitoring..._

#### CloudWatch

* CloudWatch is for monitoring performance (metrics, logs)
* Can install custom metrics on AWS

#### Systems Manager

* Used for configuration management
* Uses "Run Command" on difference EC2's

// TODO: 👷‍♀ What's the difference between systems manager and ansible etc?

#### CloudTrail

// TODO: 👷‍♀ What is the purpose of a CloudTrail? When would you use it?

#### Athena vs Macie

**Athena:**
- Serverless (so you pay per query)
- Works with data in S3
- No complex ETL

**Macie:**
- Uses NLP to protect PII on S3

// TODO: 👷‍♀ Read more about Macie, and what it is

## 4. Miscellaneous Notes

#### Linux

* `sudo su` —> Stands for switch user
* Fedora / RedHat / CentOS
    * Installs with: `apt-get`
* Ubuntu/Debian
    * Installs with: `yum`

#### DNS + S3 Websites

* Register a domain within the AWS console
* Creating a website bucket must be the same, i.e `yourwebsite.com`
