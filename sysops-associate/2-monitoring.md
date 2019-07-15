

## EC2 MONITORING

### Custom vs Out-Of-The-Box Metrics
- Standard metric time for EC2 cloudwatch metrics is 5 minutes
- Detailed monitoring is 1 minute

### The 4 CloudWatch metrics you get out of the box
- CPU
- Network
- Disk
- Status Check

**Note:** RAM is a custom metric

### Setting up a Custom Metric
- Install the cloudwatch agent on your machine (using user data startup script)
- You use a user data script to configure your EC2
- User data scripts must provider their interpreter (Use a shebang (#!/bin/bash)
- Scripts are executed as root
- On Ubuntu you can run this as a crontab (put a shell script into a cron folder)

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

## EBS Monitoring

### The Four Volume Types
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


## ELB Monitoring

### ELB overview
- ELB have listeners (for connections using defined protocol and ports)
- Register health checks on the ELB to ensure that service health (before routing)
- You can load balance across regular instances and serverless
- By default load balancers distribute across specified AZ’s

### The Types of ELB
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

# Monitoring ELB
- Cloud Watch
    - App 4/5xx’s
    - Healthy hosts count
- Access Logs (are disabled by default)
    - Dumps to S3
    - Can be hard (require 3rd party tool)
    - They’re good because they can access data after an ELB scales down
- Request Tracing
    - Adds a header to requests before sending on (so they can be correlated)

## CloudWatch (General)

### Custom Dashboards
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

## ElastiCache Monitoring

### The 3 Metrics
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


## AWS Config

### What is AWS Config?
- Management tool
- Is regional (needs to be turned on a per region basis)
- Records configuration changes
- Can schedule a lambda to be triggered on config change
- Config is a way of recording your AWS environments (config management)
- Pipes results into S3 for analysis later
- Can trigger periodically or on change
- You can have up to 40 managed rules
- Config needs read access to resources it’s tracking
- Needs publish access to SNS to trigger notifications

AWS Config FAQs
https://aws.amazon.com/config/faq/
