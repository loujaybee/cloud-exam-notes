## CloudWatch

Understanding the metrics for EC2 from CloudWatch.

* CloudWatch monitors application performance
* CloudWatch logs are stored indefinitely
* Standard metric time for EC2 cloudwatch metrics varies between 1 / 3 or 5 minutes
* Can be used on premise by using the SSM agent

##### CloudWatch concepts

- **Namespace** Is a grouping of metrics, designed to avoid clashing.
- **Metrics** Are the fundamental of your time series data.
- **Timestamp** Can be created 2 weeks in the past, or a couple of days in future.
- **Retention** Metrics are rolled up to greater resolutions over time.
- **Dimensions** A metric can have up to 10 dimensions.

### EC2 Metrics

- You can receive get metrics after termination
- When turning on detailed monitoring you can see the metrics immediately and historically (TODO: Check this)

#### Out Of The Box metrics
- CPU
- Network
- Disk
- Status Check

_RAM Utilisation is not an out-of-the-box metric_

#### Custom EC2 Metrics
- Assign an IAM role to your EC2
- Install the cloudwatch agent on your machine (using user data startup script)
- User data scripts must provider their interpreter (Use a shebang (#!/bin/bash)
- On Ubuntu you can run this as a crontab (put a shell script into a `/etc/crontab` file)

#### Detailed Monitoring
- High resolution monitoring allows faster resolution (can be as low as 1 second)
- Detailed monitoring allows greater than 5 minute granularity
- [AWS Docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html)

### The Four EC2 Pricing Models
- On Demand
- Reserved
    - Has an Upfront price (fixed cost)
    - 3 year term (check this?)
    - Useful for repeating resources (day/week/year)
- Spot
    - Very low prices
- Dedicated
    - No virtualisation with other tenants
    - Good for certain licenses (Oracle)

### EBS, The 4 Volume Types
- Solid State Drives (They’re expensive, but much quicker)
- GP2
    - Is a general purpose volume (generic use cases)
    - Up to 10,000 iops
    - Performance is linked to the size of the volume
    - Allows IOPS bursting
- IO1
    - Highest performance volume
    - Mission critical stuff
    - Provisioned IOPS
    - Used for high throughput databases etc
    - Hard Disk Drives (They’re cheaper, but performance is massively reduced)
- ST1
    - Can’t be boot, quite slow still
    - Big data (non-production, but still performant)
- SC1
    - Basically for very low cost (but slow)
    - Used for large and infrequent access
    - Has a lower storage cost

### EBS Volume Pre-Warming
- Pre-warming is an optimisation to prevent cold access of EBS blocks
- There is no need to warm EBS any more unless you’re restoring from S3, where you may want to pre-warm as each accessed block will have a “first touch penalty”.
    - You can warm EBS with a bash command that touches all files internally
    - Use the dd or fio command and read all files to /dev/null as a pre-warming process.

### EBS Cloudwatch
- Read/Writeops (I/O operations per second)
    - Amount of IOPS per second
- Queue length
    - You really don’t want anything queuing

### The Four EBS Volume Statuses
- OK — Volume is running fine
- Warning — Degraded performance, pushing to queue?
- Impaired — Not working, maybe I/O is disabled?
- Insufficient Data — Not enough data!

**Applying changes:** When you modify block storage, you might need to adjust your OS to see the higher volume size. You can also apply changes whilst the EBS is running.

### ELBs
- ELB have listeners (for connections using defined protocol and ports)
- Register health checks on the ELB to ensure that service health (before routing)
- You can load balance across regular instances and serverless
- By default load balancers distribute across specified AZ’s

### Types of ELB
- Application Load Balancer
    - Supports path based routing
    - Can forward requests with modified headers (operates at application level)
- Network Load Balancer
    - Transport layer routing (TCP/SSL)
    - Very fast, millions of RPS
    - Forwards requests without modifying headers (it can’t)
    - For very low latency needs
- Classic Load Balancer (to be deprecated)
    - Supports sticky sessions (using app generated cookies)
- Ephemeral ports?

### Monitoring ELB
- Cloud Watch (for 4/5xx’s, healthy hosts count)
- Access Logs (are disabled by default)
    - They dump to S3 so can be hard to analyse (require 3rd party tool)
- Request Tracing
    - They’re good because they can access data after an ELB scales down
    - Adds a header to requests before sending on (so they can be correlated)

### Cloudwatch Dashboards
- Dashboards aren’t regional specific
    - Can add lines, stacks, numbers and charts
- View data usage, memory
- Remember to save your dashboard

### Creating a Billing Alarm
- You can setup a threshold when you want to be alerted
- Create people who are to be alerted

###CloudWatch Vs CloudTrail vs Config
- Cloudwatch
    - For logs and monitoring of applications (performance monitoring)
- CloudTrail
    - Allows you to see what changes have been made to the AWS API
- Config
    - Records the state of AWS environments

### Elasticache monitoring metrics
- CPU utilisation
- Swap usage
    - You don’t want to be using the swap file
    - Usually equal to the size of your general elasticache
- Evictions
    - When an old piece of memory is swapped with a newer
- Concurrent connections
    - Find out if you are not releasing connections properly (assumed same as RDS)

### AWS Organisations
- Purpose of AWS orgs
- AWS Organisations allow you to manage multiple AWS accounts
- You can apply policies to your organisation that allow you to control access to AWS accounts
- You can automate account access / creation
- Can group accounts (departments, business functions etc)
- Used to consolidate billing across different accounts
- Can restrict what services people use

### Tags & Resource Groups
- Tags are key / value pairs (same as resources)
- Resource groups are groups of tagged resources
- AWS systems manager

### AWS Config?
- Is a management tool that records configuration changes
- Is regional (needs to be turned on a per region basis)
- Can schedule a lambda to be triggered on config change
- Pipes results into S3 for analysis later
- Can trigger periodically or on change
- You can have up to 40 managed rules
- Config needs read access to resources it’s tracking
- Needs publish access to SNS to trigger notifications

### Questions
* What is AWS Config?
* Read: https://aws.amazon.com/config/faq/
* How to setup a billing alarm?
* Understand resource groups better
* Understand AWS config better
