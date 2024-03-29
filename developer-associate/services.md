## Services

### EC2
- Describe instances has a **--dry-run** flag to test IAM permissions before executing
- Link local addresses start with `169.254` (are not forwarded by routers)
- **Instance metadata** is about your instance to configure or manage the running instance. 
- **User data** is a script provided to EC2 on boot, can be used to capture IP traffic, but easier to just use VPC flow logs

### API Gateway
- For lambda integration: `INTEGRATION_FAILURE` if the response type is unspecified, `INTEGRATION_TIMEOUT` if the integration times out
- `504` is a gateway timeout
- Clients can use the `Cache-Control: max-age=0` header to override cache set on the API Gateway, by using `Require authorization` setting only certain clients can invalidate the cache
- Use `IntegrationLatency` to measure integration of latency to a backend or `Latency` to measure the overall latency of the API Gateway and it's integration
- Use **Stage Variables** which act like environment variables at different API gateway stages, can be used to map the URL's of your API Gateway stages

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
- **Flow logs:** diagnose security group rules, monitor traffic, traffic direction to/from network interfaces


### RDS
- MySQL error log, slow query log, and the general log

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
- When sharding, the number of instances should match the number of KCL workers.

### ECS
- **random** - self explanatory, **binpack** - tasks are placed on container instances to leave the most unused CPU or memory, **spread** - evenly placed across availability zones

### SQS
- A queuing system decouples system via events

### Elasticache (Memcached, Redis)
- **Replication:** Redis supports replication, memcached does not
- Seems like Redis is often the more highly available option

### [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)

- Used for creating an audit trail
- CloudTrail is logging by default and will has 90 day retention by default
- If you need more than 90 days you need a **trail**. "Trails" are essentially streams that go to S3 and need to be analysed by something like Athena. Trail options (all **regions** delivered to S3 which is the default option, single region which can only be viewed in that region, **encryption**, apply **organisationally** but must be created in the management account for this and can be viewed by organisation member accounts, and can enable log file validation)
- You can send CloudTrail to CloudWatch
- **Management events** configuring security, logging setup, cannot be turned off vs **data events** which are tracking more fine-grained events, and can be enabled, only lambda and s3 are supported, but they're incredibly high volume and **insight events** which are AI generated when something suspicious appears to be happening.
- We now have **CloudTrail Lake** immutable

### Lambda 
- Reserve concurrency to prevent other Lambda's in the account taking your compute units
- You can use an Alias in Lambda to point to different code, for easy rollback. You can use the `routing-config` parameter of the AWS Lambda alias to allow you to point to two different versions of the Lambda function, this is easier than having two endpoints and doing Route53 weighted routing, for instance.
- Lambda@Edge 

### DynamoDB ([Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html))
- Two read consistency options: **Eventually consistent reads** (default), **strongly consistent** reads.
- Partitions have a maximum of 3000 RCU's (read capacity units) and 1000 WCU's (write capacity units)
- DynamoDB automatically partitions
- Primary keys (cannot be changed after table creation): **partitionkey** which must be unique or a composite primary key, e.g. **partition key** and **sort key** where sort key must be unique.
- Querying: **condition expressions** are used for `PutItem`, `UpdateItem`, `DeleteItem`, **filter expression** is filtering items, not attributes
- Avoid scans where possible, reduce page size if you can
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
- **StreamViewType:** `KEYS_ONLY`, `NEW_IMAGE`, `OLD_IMAGE`, `NEW_AND_OLD_IMAGES`


### KMS
- **Envelope encryption** is encrypting with a data key, and then encrypting with another key (assymetric and symmetric)
- KMS stores master keys, not data keys
- `GenerateDataKeyWithoutPlaintext` creates a data key without the plain text version.

### AWS SAM
- Extension of CloudFormation (with CLI)
- Also a CloudFormation Macro (a DSL)

### S3 encryption at rest
- `x-amz-server-side-encryption`
- Origin Access Identity is associated with a CloudFront distribution to restrict direct access

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

- **Ref** returns the value of the specified parameter, or resource
- **Fn:GetAttr** returns the value of an attribute from a resource in the template
- **cloudformation package** - packages local artifacts, replaces references to local artifacts

### AWS Step Functions
- A state machine for serverless workflows
- States are configured through Amazon States Language (JSON)
- Standard vs express workflows
- States:
    - **Pass state** - Which passes input through to output.
    - **Task state** - Uses a Lambda, or passing parameters to other AWS services (e.g. Lambda, DynamoDB, ECS, SNS, SQS, EMR)
    - **Activity state** - Work is offloaded to a worker and polls for update e.g. EC2, ECS
    - **Parallel state** - Parallel executions
    - **Choice state** - Decide if it goes to the next step (or not), AND/OR
    - **Wait state** - Wait a fixed amount of time, or until a certain time
    - **Map** - Iterate over an array of values
    - **Succeed/fail state** - For used with choice states that want a success/fail
- **Task state:**
    - **InputPath** - Chooses what to pass to the task
    - **Parameters** - Key value pairs to be passed as input
    - **Result Selector** - Manipulate a states rseult before the ResultPath is applied
    - **ResultPath** - combination of the state input and the task result to pass to the output
    - **OutputPath** - filter the JSON output to further limit information passed to the output
- Seems very coupled to AWS, I wonder about how we can keep this agnostic

### CodeBuild
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
- Can manage traffic shifting, e.g. doing **canary** applies pre and post hook tests and CloudWatch alarms, blue/green type deployments, and in-place deployments
- Provide bash scripts for all the hooks (app stop, before install, after install), which are zipped up
- Hooks are different depending if you're using lambda, ec2, ecs, you can see progress / result of hooks, but it won't show you the logs from EC2, so you ideally should hook it up to CloudWatch
- Can deploy on EC2, on-premise, lambda or ECS
- **EC2**: `CodeDeployDefault.HalfAtATime`, `CodeDeployDefault.OneAtATime`, `CodeDeployDefault.AllAtOnce` only applies to EC2, for **Lambda**: `CodeDeployDefault.LambdaAllAtOnce`
- Components: **Deployment Groups** (resources to deploy to), **Deployment** (apply a revision), **Configuration** (what to do during deployment e.g. success/failure), **AppSpec file** (server definition), revision (everything required to deploy a new version).

### Code Pipeline
- Continuous Delivery service (integrates CodeCommit, CodeBuild and CodeDeploy)
- A **pipeline** encompasses all components, a **stage** is a step in the pipeline, **action** does something, e.g. pull code, run tests, deploy to prod, **artifact** is created (optionally) for each **action**.
- Many **actions** integrate with AWS, but you can also create custom action

### Code Star
- Wizard for getting common application projects set up
- Has project management, issue tracking
- Three components of CodeStar: Project dashboard, deployment pipeline, access management

### Other services
- **AWS Data Pipeline** Used for automating the movement and transformation of data between different AWS services
- **AWS OpsWorks** Configuration management service (for managed Chef/Puppet)

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