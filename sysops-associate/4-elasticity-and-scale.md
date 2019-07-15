
## Elasticity & Scalability

### Elasticity vs Scalability

- **Scale:** Ability to meet demands over the long term (think: long term, months / years)
- **Elasticity:** Stretch and retract infrastructure based on demand (think: hours or days)

### Aurora & Scale
- Proprietary database that AWS invented
- Aurora allows elasticity (regular RDS does not)

### RDS Replica Vs Multiple AZ
Worth calling out on it’s own. Read replica’s are to help with additional read load whereas multiple AZ is to help with disaster recover.

### RDS & Multi AZ Failover
- Backups are taken from a secondary if you’ve got multiple AZ turned on.
- You can force a failover by re-booting (for chaos eng)  your instance.
- Connection string points to the DB instance and managed privately by AWS.
- RDS will detect if a database is down and point the DNS to a different AZ
- Multi AZ has an exact copy of your data in the other AZ (sometimes physical replication and sometimes - logical replication)
- Failover priority (set the instance you want to become the master in failover)

### Read Replicas (for performance / read-heavy loads)
- Helps with easing read-heavy workloads (such as wordpress)
- AWS takes a snapshot of the primary DB (with no multi AZ it causes I/O suspension)
- Easily have multiple instances of your RDS (through the interface)
- Data is automatically synced between databases
- You can know which RDS you’re running (by viewing the “engine” field)
- Can direct traffic to reads if the write replica is under scale
- Useful for reporting such as data warehousing (or could use S3 and redshift)
- Creates a new endpoint DNS record to connect to
- You can promote a read replica to become a primary DB if you want
- Key metric: Replication lag (the time taken to replicate between databases)

## ElastiCache
- A web service that makes it easy to deploy, operate and scale an in-memory cache in the cloud. Much - faster than a disk-based database.
- Can significantly improve latency and throughput for many read-heavy application workloads (social - networking, gaming, media sharing, Q&A portals)
- Improves performance by storing critical pieces of data in memory, for low latency access.

### Memcached vs Redis
Memcache does not support multi AZ, Redis does support multiple AZ.

### Aurora

Serverless database solution (can be provisioned if you want)

### Troubleshooting Autoscaling

Below are some reasons that instances might not be able to autoscale...

- Security group doesn’t exist
- Instance type is not supported in the AZ
- AutoScaling group doesn’t exist
- Invalid EBS mapping
