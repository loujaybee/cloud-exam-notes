
## Regions, Availability Zones, and Edge Locations

Breakdown and understanding of groupings within AWS...

**Regions**
* Rough geographical region

**Availability Zones**
* At the AZ level, you're protected against hardware failures
* An AZ is a logical AZ zone
* An AZ is a logical construct
* An AZ is not necessarily a single data-center
* It's a logical fault-tolerant logical grouping
* AZ's are fiber connected (so they're very fast)

**Edge Locations**
* Where individual AZ's would be overkill
* Act like mini availability zones

**VPC**
* A VPC can live within one *_REGION_*
* Internet gateways can be attached to VPC's to access the public internet

**Subnet**
* One subnet lives in a single AZ (the AZ is picked when you create the subnet and cannot be changed!)

## Creating a VPC
* CIDR + Tenancy (Region?) cannot be changed once created
* CIDR is between /16 and /28 (65,000 —> 16 IP's)
* Overlapping VPC's have issues integrating / peering (default use same range, be careful)
* Chop up your VPC to be shared between AZ's
* Leave some spare VPC space to be added to new AZ's if released

## Reserved Addresses
* First, Network address (10.0.0.0)
* Second, VPC Router (10.0.0.1)
* Third, DNS (10.0.0.2)
* Reserved, in case (10.0.0.3)
* Broadcast address (10.0.0.255)

## ENI
* Is an abstraction between network config and instances
* An ENI is provisioned onto an EC2
* Scoped into an AZ
* A security group is attached to an ENI, not an instance
* Larger instances can have multiple ENI's
* ENI's given at creation time, and those given later have different behaviours
* [Information stored by an ENI:](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html)
    * MAC address
    * Private IP
    * Elastic IP
    * >=1 security groups
    * One public IP address
    * A source/destination check

## Elastic IP
* External, public facing IP's
* Elastic IP's are linked to private IP's

## Internet Gateway
* Is region specific
* Attached to a VPC
* Allows access to public endpoints
* VPC Router communicates with the IG
* Non Internal addresses are routed to the Int3ernet Gateaway
* Usually lives at 0.0.0.0

## Security Group
* Applied to EC2, DB, etc (really to the ENI)
* Are scoped to a VPC
* Can only allow traffic (no deny)
* Cannot block IP from in a security group (only in NACL)

## NACL
* Applied at the Subnet level (not the instance level)
* Can be applied to many subnets
* Acts like a firewall
* Is state-less, doesn't remember packets previously came in or out
* NACL rules are processed in order (no rejection)
* The default ACL is given an allow all rule to get people started

## Questions
* How can CIDR blocks overlap?
* What issues would it cause if two VPC's have the same IP address ranges?
* Reserved instances are applied to VPC's and subnets?
* "Broadcast is not supported in a VPC?" what does this mean?
* What is DHCP?
* What does CIDR stand for?
* What is a source/destination check?
* Internet gateway vs NAT instance
* How do you debug network issues
* NACL vs Subnet
* Do you hit every in the NACL?
* What is an emphemeral port?
