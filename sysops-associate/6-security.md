# Security & Compliance

## MFA
* Turn on MFA through the IAM console
* You can save your QR code from the MFA
* MFA for the command line you just use STS for the token
* There is a report within IAM that shows you who has MFA enabled

## DDOS
* AWS Shield covers CloudFront, Route53 and ELB's
* Use autoscaling to help consume attacks
* Use CloudWatch to alert if an attack is ongoing

## Security Marketplace
* Different security products, like Kali Linux
* Can buy ACloudGuru training
* Can buy hardened images (to certain standards)

## STS (Security Token Service)
* Used for creating access tokens, returns your ```ACCESS_TOKEN``` and ```SECRET_TOKEN```
* Can authenticate against a third party (facebook, google, etc)

## Security Logging
* CloudWatch monitors performance
* CloudTrail monitors API calls (You can hook into Lambda's or raise alerts based on these)
* AWS Config records current state of environments and shows changes

## Systems Manager (Run Command)
* Apply security patches to a fleet of EC2
* Easily run

## IAM Custom Policies
* Can create custom policies that are applied to users / roles.
* Some policies are AWS managed, some are user managed.
* Actions are different things you can do with a resource.
* Can be attached to an EC2 instance.

## Pen Test
* You need to notify AWS if you're performing a pen test

## AWS Hypervisors
* Abstraction between machine and running image

## Shared responsibility Model
* The 3 types of services: Infrastructure, Container and Abstracted services

## Question

* How does it detect DDOS?
* The difference between run command and chef / puppet (and OpsWorks?)
* Why notify AWS of pen test?
* Can we apply policies to all resources?
* How do root privledges work? "Elevate to root?"
* How do attached roles on machines assume the role details via the SDK?
* What does "Managed policy" mean within AWS?
