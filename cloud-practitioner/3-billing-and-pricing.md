# Billing & Pricing

## 1. Billing Overview

General billing notes...

#### Capex vs Opex
* **Capex**: Fixed upfront cost (static hosting)
* **Opex:** Operational expenditure

#### 4 Pricing Principles
1. Pay as you go
1. Pay for what you use
1. Pay less as you use more
1. Pay even less when you reserve capacity

#### 5 Basic Pricing Policy
1. Pay as you go
1. Pay less when you reserve
1. Pay even less per unit by using more
1. Pay less when AWS grows
1. Custom pricing

#### The 3 Drivers Of Cost
1. Compute
1. Storage
1. Data (Outbound)

#### The 4 Pricing Models

* On Demand (pay for what you use)
* Reserved (reserved for a contract time)
* Spot (based on market price)
* Dedicated Hosts (for your use)

#### What is free?
* VPC is free
* Elastic Beanstalk
* CloudFormation
* IAM
* AutoScaling

## 2. What Determines Pricing?
You need to know the factors that affect pricing for the main services.

#### For EC2...
* Server Time
* Instance Type
* Pricing Model
* Number of Instances
* Load Balancing
* Detailed Monitoring (polling every 1 minute)
* Elastic IP's are paid

#### For Lambda...
1. Per request ($0.20 per million requests)
1. Duration price
1. Data transfer cost (such as data in/out of S3)

#### For EBS...
* Volumes
* Snapshots
* Data Transfer

#### For S3...
1. Storage Class
1. Storage (Amount)
1. Requests
1. Data Transfer (data in/out)

#### For Glacier...
1. Storage
1. Data Retrieval Time

#### For Snowball...
* 50TB $200
* 80TB $250
* First 10 days free (after that $15 a day)

#### For RDS...
* Time of server uptime
* Instance Type
* Number of instances
* Storage (Provisioned / Additional)
* Requests (and data transfer)

#### DynamoDB
* Write / Read
* Amount Of Data Stored In DynamoDB

## 3. Billing Services / Tools

### Budgets vs Cost Explorer

- **Budgets**: Forecasts costs _before_ they occur.
- **Cost Explorer**: Costs _after_ they've occurred.

### Tags

- Tags & Resource Groups
- Can Create Resource Groups
