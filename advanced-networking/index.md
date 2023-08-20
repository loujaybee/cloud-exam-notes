## Regions + Availability Zones

## DNS 

- You can use Route53 for DNS instead of IP if wanted
- You can use CloudFront for additional caching

## VPC

- A fully controlled network by you (resembles an on-prem network)
- You cannot use a VPC without a subnet
- A VPC is scoped to a single region
- You can enable VPC flow logs for audit of network access
- Managed service without VPC: S3, DynamoDB, AWS Lambda, API Gateway, SQS, SNS

#### CIDR (Classless Interdomain Routing)

- https://cidr.xyz/
- https://www.ipaddressguide.com/cidr
- Syntax is the IP range and CIDR prefix `X.X.X.X/X` and the formula for the address range is: `2 ^ (32 - Prefix)`
- AWS doesn't use class based routing (Class A, Class B, Class C) it uses the subnet mask prefix, e.g. `/26`
- Considering IPv4 has 32 bits, for example `/16` means there are 16 bits available which is 2^16 which is 65,536. In short, the larger the prefix the fewer the available addresses for the network. 

## Availability Zone

- Use at least 2 availability zone
- Subnets are in the AZ

## Nat Gateway

- By default is created in a public subnet
- Is AWS managed and scales automatically
- Replaces the source IP addresses of the origin to it's own
- Lives in the public subnet(?)

## Subnet

- Exist in a single availability zone
- There can be no overlapping IP addresses
- Public subnets have internet gateway access
- A load balancer can route to the private network

### Route Table

- Is the attachment of the internet gateway
- Has IP of entire VPC defined as the "local" network
- Can attach to multiple subnets

### NACL

- You need to define in and outbound
- NACLs are stateless (doesn't remember traffic in and out)
- Is the "2nd layer of defence"
- Can attach to multiple subnets

### Security Group

- Security groups are stateful (inbound and outbound)
- All rules will be evaluated
- There is no "DENY" in security groups

## Internet Gateway

- You'll need an internet gateway in a subnet

## VPC Endpoint (Interface and Gateway)

- VPC Endpoint Interface for Lambda SQS and SNS
- VPC Endpoint Gateway for S3 and DynamoDB
- Avoids the public internet for networking
- You can connect to private resources like S3 and DynamoDB
- S3 and DynamoDB has to be in the same region

## VPC Peering 

- Can reach from any machine to any machine
- Connections are non-transitive
- Private connectivity between two VPCs
- VPC Peering flows through the AWS managed network
- Flows through AWS managed network

## Private Link

- Connects a private (on prem?) VPC to an AWS VPC
- Expose VPC Endpoint to Network Load Balancer of Private VPC

## Transit Gateway

- Solves the issue of transitive VPCs
- Any VPC can connect to any VPC
- Useful for hybrid connections
- Also can be better than individual VPNs

## Virtual Private Gateway

- Goes over the internet via VPN
- Is called "site-to-site" connection

## Direct Connect

- Is used as a substitute to Virtual Gateway + VPN
- One of the most important service
- A physical link to your corporate data center

## Notes

- VPC has the larger block of addresses (CIDR block)
- Private/Public resources
    - Lots of services are private
    - You can control the resources from the public console, but resources can be private
- NACL (stateless firewalls, in and out traffic)
    - Subnet level firewalls
- Security Group
    - Stateful firewall (traffic that leaves and returns)
- Firewall (Stateful vs Stateless)
    - Stateful connects in and outbound requests
    - Stateless doesn’t connect inbound and outbound requests
- VPC Peering links
    - Are not transitive (can’t go via another VPC)
- IAM
    - Principal (person or application)
- AWS Local Zone
- Transit Gateway
    - VPC peering
    - Can be attached to VPN, Direct Connect
- Amazon VPN (site-to-site)
    - Customer Gateway deployed on the customer side
    - Establish a connection between the on-prem customer gateway
- AWS Wavelength Zone
    - 
- Virtual Gateway
    - 
- AWS Outposts
    - Can run EC2, EBS, S3, VPC + RDS
- VPN CloudHub
    - Goes over the public internet
- AWS Direct Connect Location
    - Like a hopping point into AWS
    - Can help improve performance (more expensive than VPN)

## Further Reading

- What is AWS Direct Connect?
- Are there any networking projects?
- Difference between NACL and Route Table? 