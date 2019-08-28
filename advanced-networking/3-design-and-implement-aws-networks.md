
### Regions, Availability Zones, and Edge Locations

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
