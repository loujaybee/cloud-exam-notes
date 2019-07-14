Part 7a: Networking (VPC)
Need to be able to build out a VPC from scratch (and memory)
What is a VPC?
A VPC can be thought of as a virtual data center in the cloud
A VPC is logically isolated and completely controls IP’s and gateways
A VPC can have public and private networks
A VPC can be between on prem and virtual
You can have 5 VPC’s per account on account creation (you can ask for more)
You can only attach one internet gateway to a VPC
Allows you fine-grained control of your network
Connecting to a VPC
There are two main ways to connect to a VPC:
Via an Internet Gateway
Via a Virtual Private Gateway (Apparently this is a VPN really?)
Components of  a VPC (Subnet, AZ, Route Table, NACL, VPG, Security Groups)
Subnets live in Availability Zones
Security groups are then assigned —> subnets
Route tables are what connects subnets
A NACL (Network Access Control List)
Security groups are stateful
VPC Peering
You can connect a VPC with another (through peering)
VPC’s then behave like they’re on the same network
VPC’s are not transitatively peered (peers of peers do not have access)
Peering is always hub and spoke (one central hub, with many peers)
Creating a VPC
Under Network & Content Delivery
Choose your CIDR block range
Creating a VPC creates the following:
A route table
A Network Access Control List (NACL)
Allowing all inbound traffic by default
Creating a VPC does not create...
Subnets
Creating subnets
Set the name
Set the address range
Set the AZ (subnets must live in AZ’s)
Create an internet gateway
Needs to be attached manually to your VPC
You can have one internet gateway per VPC (no more!)
Create a security group (for a private resource such as a DB)
Set the source as custom (from your CIDR address range)
SSH / HTTP / HTTPS / ICMP
AWS IP address ranges
First 4 and last 1 IP’s are reserved for different reasons.
What is a NAT gateway?
A NAT instance is a way to get traffic from a private network out to the internet.
NAT instances are useful for things like database patching (which is required on private network machines)
NAT instances vs NAT gateways
You can create a NAT instance from an amazon AMI
Disable source / destination check for the NAT (A NAT is not the source or destination of traffic)
You create a route table for your NAT gateway which proxies traffic out to the internet from a private instance
Would need to monitor bandwidth (like a typical instance)
Can also use your NAT gateway as a Bastion (seems like a bit of a hack to me)
EGRESS only internet gateway
Nat Gateway
Don’t have to worry about patching OS
Supported up to 10GB/s
Is a component that is in VPC panel within AWS
Built to be highly available (need one in each AZ for high availability)
READ: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html
Question: Do they always have to be EC2 instances or is there an AWS component?
Question: What’s the difference between a NAT gateway and a NAT instance?
Network ACL’s and Security Groups
What is a NACL?
Add ACL rules with increments of 100 (to give you room in between)
Allow internet from anywhere with 0.0.0.0/0
You can only associate a subnet to a single Network Access Control List
Newly created private NACL’s will be closed off to the internet (unlike ones given out of the box when you created a new VPC)
You need to associate your NACL with your subnet
Rules for NACL’s are evaluated in numerical order
You should number each rule in increments of 100 (so that you’ve got some wiggle room)
NACL’s are a way to block specific IP’s and IP ranges.
NACL’s are assessed before security groups
VPC comes with a default NACL (that allows all inbound and outbound traffic)
Custom written NACL’s allow no traffic in or out
Not associating a subnet with an NACL
Network ACL’s are stateless
You can block IP’s using ACL’s not security groups
VPC Endpoints
Keeps traffic in the private network (without going over the public)
Basically two AWS resources can talk to each other
VPC Flow Logs
Go to your VPC and toggle on if you want
IP traffic going to and from network interfaces in a VPC
Can be at different levels:
VPC
Subnet
Network interface level
Can stream logs to Lambda (so you can proactively react) or elastic search
Note: Doesn’t log all traffic as some is between AWS resources (Route 53, VPC router etc)
VPC Clean Up
Clean up all resources first (such as EC2’s)
Delete NAT Gateways
Delete Internet Gateways
Delete VPC Endpoints

Part 7b: Networking (DNS)
TLD’s are controlled by the IANA. Route 53 naming — DNS works on route 53, which is the origin of the name. The TTL is the time that the resource lives, you want this to be low if you’re making changes.
Types of DNS records
SOA
Name of the server that supplied the data
NS
Domain registrar points to the name servers (where the routing records are)
Multiple TLD NS records (to protect from outages)
A
Most fundamental type of record
Stands for address
Simply translates the name to an address
CNAME
Resolves one domain to another
These cannot be used for naked domain names
ALIAS (unique to Route53)
Map record sets to elastic load balancers
Can be used for naked domains
Useful as Amazon can update resource records (so you don’t have to)
Basically AWS manage the IP to host mapping!
Routing Policies
Simple
Can only have a single record
If you want multiple values, you can, but they’re chosen at random
Despite being random, it could be cached at DNS, so you’ll get the same
Weighted Routing
When you have two resources (and want a failover) you assign a weighted policy for speed, but have the second as fallback.
Weighting: A number (these are all added up) i.e 20 + 20 = 50% each.
Set ID: Unique value for that weighted record
Latency Based Routing
Can’t have latency or weighted together
Route based on lowest latency region
Failover Based Routing
How you can setup active/passive routing
Uses a health check to ensure that your site is up in a given region
Create a health check from within Route53 (didn’t know you could do this!)
Geolocation Routing Policy
Point different location based customers to different sites
Multivalue answer
Balances across two resources
