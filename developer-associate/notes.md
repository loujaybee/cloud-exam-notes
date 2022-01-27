
## Overview
- **Days to go**: 16
- **Percentage remaining**: 94%

## Todo
- Run through: 
  - Networking services
  - The "DevOps" services
  - Core services (Lambda, EC2, RDS, etc)
- Update your imaginary business repo with some new services
  - Create an AWS diagram
- Check out the other courses to see if anything is missing (ACloudGuru, Stephane Mareek, Cloud Academy)
- Buy prep questions

## Prep Plan
- Monday AM - Run through main services you don’t know (EB) + do recap
- Monday PM - Recap Lesser Known Services (Step Functions, API Gateway, Code*) + do recap
- Tuesday AM - Networking services / VPC + do recap
- Wednesday AM - Recap Least Known Services + do recap
- Wednesday PM - Recap Least Known Services + do recap
- Thursday AM - Cheat Sheets, Flash Cards, Exam Questions, Practice Exam on ExamPro, Review whitepapers
- Thursday PM - Practice Exam on ExamPro + Jon Bonso

## Courses
- ExamPro: https://app.exampro.co/student/material/dva-c01/1188
- Stephane Mareek: https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/learn/lecture/19733666?start=15#overview
- Exam Questions: https://twitter.com/nealkdavis/status/1484468958090596358?s=20

## Concepts

### CI/CD/CD
- Continuous Integration: Code, Build, Integrate, Test
- Continuous Delivery: Code, Build, Integrate Test, Release (preparing code for deployment, but deployment is still a manual process)
- Continuous Deployment: Code, Build, Integrate Test, Release, Deployment 

## Services

### VPC ([Docs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html))
- **Stats:** 5 VPC's per region (VPC's are regional, not AZ), 200 subnets per VPC
- **Free:** (VPC, Route Table, NACL, Internet Gateway, Security Groups, Subnets, VPC Peering)
- **Paid:** NAT Gateway, VPC endpoints, VPN gateway, customer gateways
- **Default VPC:** Internet gateway, route table, 
- `0.0.0.0/0` represents all possible routes
- **VPC peering** - No overlapping CIDR blocks, no transitative peering, connects same accuonts and/or regions
- **Internet gateway** - Allows VPC to access internet, provides a target in the route table for routable traffic, and/or performs NAT for instances assigned a public IPv4 address
- **Bastion** - Should be deployed on a hardened image, however systems manager session manager is used instead of bastions in AWS
- **Direct connect** - Is very fast, provides a dedicated network connection, which avoids the internet
- **Security Groups** - Firewall at the instance level and provide port and protocol rules. 
- **NACL** - Security groups are at the instance level, whilst NACL's are at the subnet level (a subnet has one NACL, one NACL can have many subnets) slight differences such as ALLOW vs DENY lists, and application of firewalls, NACL's allow blocking of IP ranges, not only port / protocol. 

### Elastic Beanstalk ([Docs](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html))
- Not recommended for production applications
- Fancy CloudFormation template
- Go, Node.JS, Java, Python, Ruby, PHP (typically comes with the)
- **Web** single instance environment (with ASG) w/ direct IP access vs load balanced **Worker** (With Queues)
- Deployment policies: **all at once** deploy to all instances at the same time, **rolling** deploys to batches of existing services at a time, **rolling with additional batch** launch new servers, and terminate old servers (never reduce old capacity) **immutable** relies on the auto-scaling group by creating new instances, and re-pointing the ASG, rollback is done via swapping back, **blue/green** is swapped at the DNS (environment) level, not within the environment. 
- You can use configurations to override default values in Elastic Beanstalk: `env.yml` sets the environment name, the solution stack, **Linux Server** which sets up your machine (an alternative to an AMI/Docker?)
- **CLI** - view logs, create an environment
- **Database** - Create an RDS database *inside* or *outside* elastic beanstalk. 

### Kinesis
- Real time data streaming service (sharded streams)
- Stream types: **data streams** pay per running shard, consumers keep their own position, data is ordered, persist in a stream for a long time (up to 365 days, defaults to 24 hours), **data analytics streams** allow you to run SQL on the stream for real-time data analytics, **video streams** allows you to process a video stream (applying ML or processing on the stream), **firehose** is simpler, data immediately dissapears and can only have one consumer.

### SQS
- A queuing system decouples system via events

### [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- Used for creating an audit trail
- CloudTrail is logging by default and will has 90 day retention by default
- If you need more than 90 days you need a **trail**. "Trails" are essentially streams that go to S3 and need to be analysed by something like Athena. Trail options (all **regions** delivered to S3 which is the default option, single region which can only be viewed in that region, **encryption**, apply **organisationally** but must be created in the management account for this and can be viewed by organisation member accounts, and can enable log file validation)
- You can send CloudTrail to CloudWatch
- **Management events** configuring security, logging setup, cannot be turned off vs **data events** which are tracking more fine-grained events, and can be enabled, only lambda and s3 are supported, but they're incredibly high volume and **insight events** which are AI generated when something suspicious appears to be happening.
- We now have **CloudTrail Lake** immutable

### DynamoDB ([Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html))

- Two read consistency options: **Eventually consistent reads** (default), **strongly consistent** reads.
- Partitions have a maximum of 3000 RCU's (read capacity units) and 1000 WCU's (write capacity units)
- DynamoDB automatically partitions
- Primary keys (cannot be changed after table creation): **partitionkey** which must be unique or a composite primary key, e.g. **partition key** and **sort key** where sort key must be unique.
- Avoid scans where possible
- Choose **provisioned** (limit the maximum amount of capacity where throttled requests are dropped) or **on-demand** capacity where traffic is unpredictable or scales to zero (default upper limit is 40,000 WCU's/RCU's)
- **RCU** - 1 RPS (strong consistency). 2 RPS (eventual consistency) per 4KB in size
- **WCU** - 1 WPS up to 1KB
- For global tables (multi-region, multi-master) you need KMS, and streams enabled
- Transaction support (performs two underlying reads/writes) via `TransactWriteItems` and `TransactGetItems`
- TTL for expiring a record (you have to specify the attribute/column with a numerical date/epoch)
- **Streams** capture changes (write, update, delete) to data in the order of transactions
- **Errors** - **ProvisionedThroughputExceededException** where you exceed provisioned throughput, SDK will retry with exponential back off.
- **Indexes** are kinda like a copy, and can be used to query or scan against for different access patterns **LSI** can only be created with initial table, you can only have 5 of them, can be queried in eventual or strong consistency and consume resources from the base table **GSI** are limited to 20 per table, can be updated at any time and are stored away from the base table (so they scale separately).
- **DAX** - Fully managed in-memory cache for read-intensive applications to reduce response times

### AWS SAM
- Extension of CloudFormation (with CLI)
- Also a CloudFormation Macro (a DSL)

### Parameter Store (AWS Systems Manager)
- Store data like passwords, database
- Create hierarchies like directories (using forward slashes)
- Can encrypt the string with KMS
- Free up to 10,000 parameter fetches
- Pricing: Standard and Advanced (parameter policies)
- Parameter policies allow you to emit triggers e.g. notifications on parameter changes
- Parameter policies: Expiration, NoChangeNotification, ExpirationNotification
- Everything in parameter store is automatically versioned
- You can't revert an advanced parameter due to it's size

### Amazon Cognito
- **Cognito User Pools** - User directories that provide sign-up and sign-in options for your apps users. Manage actions like sign-up, sign-in, account recovery, confirmation. Works via JWT. Can integrate events with Lambda. 
- **Cognito Identity Pools** - Grant temporary access to other AWS services.
- **Cognito Sync** - Syncs user data and preferences across multiple devices (uses SNS under the hood). Is key-value data. 
- Amplify CLI provisions cognito under the hood 

### AWS CloudFormation
- Macro's are DSL's that provide extensions to CloudFormation. Macro's are created via AWS Lambda. 

### AWS Step Functions
- A state machine for serverless workflows
- States are configured through Amazon States Language (JSON)
- Standard vs express workflows
- States:
    - **Pass state** - Which passes input through to output.
    - **Task state** - Uses a Lambda, or passing parameters to other AWS services.
    - **Activity state** - Work is offloaded to a worker, e.g. EC2, ECS, etc. 
    - **Parallel state** - 
- Seems very coupled to AWS, I wonder about how we can keep this agnostic

### Code Build
- Works via a `buildspec.yml` declarative build file (but you can override with the CLI)
- Buildspec versions: `0.1` runs each command in a separate instance of the default shell, `0.2` is recommended
- Two options for providing the build specifications: build commands, the buildspec file.
- There are some managed images to use out-of-the-box
- Supports the following: `CodeCommit`, `S3`, `GitHub`, `Bitbucket`.
- `buildspec.yml` needs to be at the root of your directory and exactly named
- **Phases:** `install`, `pre_build`, `build`, `post_build`

### Code Deploy
- Fully managed service in the cloud
- Configured using the `appspec.yml` file
- Can manage traffic shifting, e.g. doing canary, blue/green type deployments, and in-place deployments
- Provide bash scripts for all the hooks (app stop, before install, after install), which are zipped up
- Hooks are different depending if you're using lambda, ec2, ecs, you can see progress / result of hooks, but it won't show you the logs from EC2, so you ideally should hook it up to CloudWatch
- Can deploy on EC2, on-premise, lambda or ECS
- Update AWS Lambda function versions
- Components: **Deployment Groups** (resources to deploy to), **Deployment** (apply a revision), **Configuration** (what to do during deployment e.g. success/failure), **AppSpec file** (server definition), revision (everything required to deploy a new version).

### Code Pipeline
- Continuous Delivery service (integrates CodeCommit, CodeBuild and CodeDeploy)
- A **pipeline** encompasses all components, a **stage** is a step in the pipeline, **action** does something, e.g. pull code, run tests, deploy to prod, **artifact** is created (optionally) for each **action**.
- Many **actions** integrate with AWS, but you can also create custom actions

### Code Star
- Wizard for getting common application projects set up
- Has project management, issue tracking
- Three components of CodeStar: Project dashboard, deployment pipeline, access management

## Questions
- VPC: Do you have to provision internet gateways? 
- EB: Linux server configuration, why not Docker?
- EB: What can you configure with the configuration files?
- EB: Why is blue/green performed at the DNS level?
- CT: How does the cross account trail work? It tracks other accounts? How? 
- CT: How does log file validation work? Immutable log? 
- CT: CloudTrail vs RedShift, what's the big difference? (Try it)
- KSS: Difference between Kinesis and SQS
- COG: How does SAML really work?
- COG: Difference between cognito user pools vs AWS log in via SSO
- COG: What's the difference between cognito and Auth0?
- PS: Can parameter store be used for storing passwords? Why?
- CD: What's the difference between CodePipeline and Code Deploy?
- CD: Why over GitHub Actions, for instance?
- CB: How does the parameter store integration work?
- CB: Why is Code Build separate to Code Deploy?
- SF: What's the difference betweens standard and express workflows in step functions?
- SF: How are the other services invoked via the parameters? How does that work in more advanced cases?
- SF: When calling out to another service, how does it call back? Is that managed via the SDK? Or via something else?

## Hands On
- Create your own Bastion in example corp (https://github.com/openupthecloud/example-corp)
- Update example corp with networking (https://github.com/openupthecloud/example-corp)
- EC2: Use session manager to log into EC2 server
- EB: Download the EB CLI and use for AWS
- DDB: Setup different indexes and query them
- Cognito: Authenticate the user
- CD: Use CodeDeploy to deploy EC2 instances
- CB: Run a code build pipeline
- SF: Create your own step-functions example project

## Further Reading
- [Complete CI/CD with AWS CodeCommit, AWS CodeBuild, AWS CodeDeploy, and AWS CodePipeline](https://aws.amazon.com/blogs/devops/complete-ci-cd-with-aws-codecommit-aws-codebuild-aws-codedeploy-and-aws-codepipeline/)
- [The Case For And Against Cognito](https://theburningmonk.com/2021/03/the-case-for-and-against-amazon-cognito/)
- [CloudFormation Macro's](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html)
- [The DynamoDB Book](https://www.dynamodbbook.com/)
