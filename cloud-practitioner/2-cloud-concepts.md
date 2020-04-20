
# Cloud Concepts

## Using IAM Roles (with EC2)

A safer way to give priveledges to EC2 instances, if you use the AWS CLI within an instance with a role, it will get the privledges from the attached role.

1. Choose the service that will use the role.
1. Attach a policy to the role.
1. Tag the role.
1. Attach the role to the EC2.
1. Don't need credentials on the EC2, you can just use the role.

## How To Use An Elastic Load Balancer

Three types of ALB exists...

1. Application Load Balancer (Layer 7)
1. Network Load Balancer (High Performance)
1. Classic Load Balancer (Being Deprecated)

Configuring an ALB:

* You need to configure your ALB for which AZ's which it can use.
* You'll need a target group for the servers that will receive your traffic.
* Configure your polling thresholds (how often it polls the server)
* Can take ~5 minutes to setup an ALB

## EC2

- Vertical server in the Cloud
- Instance Types
    - T3, Lowest Cost, General Purpose
    - M5, General Purpose
    - X1, Large Memory

## EBS

- Storage Volumes attached to EC2 instances
- SSD type
    - GP2 - for general purpose
    - IO1 - for provisioned IOps (DB servers)
- Magnetic
    - ST1 - Lowest cost HDD (data warehouse)
    - SC1 - Lowest cost (file servers)

// TODO: 👷‍♀ Research more about these EBS types

## AutoScaling

Two things to autoscaling...

1. Launch Configuration (how you want to launch your EC2)
1. Autoscaling Group

## CloudWatch

* CloudWatch is for monitoring performance (metrics, logs)
* Can install custom metrics on AWS

## Systems Manager

* Used for configuration management
* Uses "Run Command" on difference EC2's

## Linux

* `sudo su` —> Stands for switch user
* Fedora / RedHat / CentOS
    * Installs with: `apt-get`
* Ubuntu/Debian
    * Installs with: `yum`

// TODO: What's the difference between systems manager and ansible etc?
// TODO: 👷‍♀ Experiment with autoscaling groups
// TODO: 👷‍♀ Active/Active or Active/Passive with ALB?
