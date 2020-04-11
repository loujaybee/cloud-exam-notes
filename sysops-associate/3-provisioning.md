
## Provisioning an EC2

- Choose an AMI: Redhat, Window, Pre-Installed Binaries
- Choose a VPC: One is setup as the default.
- Choosing a placement group: Allows you to physically locate machines near each other.
- Assign IAM roles: Give access to resources

### EC2 States
- Stopping vs Hibernating
    - Hibernate does no lose RAM
- Spot instances are significantly discounted (up to 60% off)
    - Will close if exceeding your max price
    - Will close if not enough instances available
    - Not good for high availability

### IAM roles
- Enable EC2 to access AWS resources
- DB or S3, for instance

### Shutdown Behaviour
- OS stop behaviour
- Define what happens when you stop an instance
- Enable termination protection
- Prevent unwanted deletion of a resource

### CloudWatch
- Metrics on a 5 minute schedule
- Detailed monitoring can be toggled (for a cost) to run every minute
- You can get more fine grained metrics with custom metrics

### Shared Tenancy
- Can configure dedicated instance (at additional cost)
- Advanced User Data
- Used for running startup commands
    - `#!/bin/bash` <— For the interpreter to know what shell to use (shebang)
Allows you to run start up scripts for your machine

### Volume Mounting

- Can mount a volume

### Security Groups

- Firewall rules
- Decide on traffic in and out of an instance

### Launch Log

- Not too sure what this is?

### EC2 Launch Issues
- InstanceLimitExceeded
    - You’ve maxed your instance count (25 default)
- InsufficientCapacityError
    - No more machines in AWS
    - Can mitigate by:
- Wait and trying again
- Request fewer machines
- Change the instance type
- Reserve instances
- Don’t specify the AZ

---

## EBS Provisioning

- Can be used for storage volumes
- Used for databases
- Used for operating systems
- Two Types of SSD
    - GP2
        - General Purpose SSD
        - Min 100 iOPS + more per GB of memory up to 16,000 IOPS max
    - Io1
        - 50 IOPS per GB
- A max of 64,000 IOPS
- 6x the IOPS of general purpose
- Running out of IOPS
    - When you run out of IOPS you create a queue
        - This can drastically slow down your application
        - Remedy: Increase volume size, Switch to provisioned IOPS

---

## Elastic Load Balancer
- Steps to deploy an ELB
    - Go to EC2
    - `#!/bin/bash (shebang for the interpreter)`
    - Add HTTP port 80 for web traffic (0.0.0.0) inbound
    - Don’t open 0.0.0.0 to everyone for SSH, instead use VPN or a custom IP
    - Create load balancer from within EC2 console
    - App load balancer using layer 4
    - Name the ALB
    - Select availability zones
    - Use security group that has inbound :80 traffic
    - Setup a health check URL (can use index.html)
    - Select registered targets for the ALB
    - ALB will then be shown as active

### 3 Types of Load Balancer
- Application Load Balancer
    - Operates at Layer 7 (application layer)
    - Content based routing (reads packets)
    - Advanced request routing (based on headers etc)
    - Specific requests go to specific servers
- Network Load Balancer
    - Operates at layer 4 (Transport)
    - TCP level load balancing
    - Ultra low latency
- Classic Load Balancer
    - Legacy (ignore largely)
    - Does both network and app load balancing
    - Research the OSI layers model
    - I’m really interested in best practices for VPC’s

## ELB Provisioning
- ELB pre-warming
- Can be done by contacting AWS
- Tell them:
- Traffic expected,
- Start and end date
- RPS and average request size
- ELB and static IP’s
- ALB scales and the IP changes
- Network load balancers can have elastic IP’s

### ELB Errors
- 400 — Malformed
- 401 — Access Denied
- 403 — Request Forbidden
- 460 — Closed connection
- 463 — X-Forwarded-For (has more than 30 IP addresses)
- 500 — Internal Server Error
- 502 — Bad gateway
- 503 — No registered target
- 504 — Gateway Timeout
- 561 — Unauthorised (ID provider)

---
## AWS Systems Manager
- Visibility and control of AWS infrastructure
- Integrates with CloudWatch
- Run commands (tasks) such as patching
- Organise inventory grouping resources together by application or environment
- Seems quite configuration management-y

### Run Command
- Can be used to stop/start an EC2
- Run playbooks (such as Ansible)
- Attach EBS volumes

### Find Resources
- Serach by tag
- Save as resource group if needed (for ease of later use)

### Insights
- Config, cloudtrail, personal health dashboard, trusted advisor

### Personal Health
- Issues with AWS
- Issues that could affect your infrastructure (AWS infra issues for instance)
- Can view all events (for all regions)

#### Trusted Advisor
- Cost optimising
- Security
- Recommended actions
- Alerts if you’re near resource limits (VPC’s etc)
- Idle resource indications

### Cloudwatch Dashboards
- Quickly see

### Inventory
- High level view of the sources

### Automation
- Automate tasks
- Back ups
- Stop instances
- Do all automation (or one by one)
- Parameter store
- Secrets + Configuration data management
- Central encryption

---

## Bastion Provisioning
- What is a Bastion?
- Also called a “jump box”.
- Typically hosted in public subnet (or open network accessible).
- Don’t expose EC2 to the internet directly.
- You should lock down Bastion to only accept traffic from single IP’s.
- Only expose SSH access (ports) on your Bastion
