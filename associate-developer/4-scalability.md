
## Scalability

- Vertical vs Horizontal scalability (scale up & down)
    - Increasing the instance size
    - Common for non-distributed system
    - RDS, Elasticache vertical scale
    - There's usually a limit to your vertical scaling (hardware limit)
- Horizontal scalability (scale in & out)
    - Add additional
    - Note: Lambda encourages horizontal scalability

## High Availability

- Usually means running in at least two data-centers
- The goal of HA is to survive data center loss

## Load Balancing

- Balances traffic between instances
- ELB is managed load balancer (you can do your own)
     - AWS take care of upgrades, maintenance, high availability
- Health checks (ping to instances)
    - Is used to help the load balancer know if the service is healthy
    -

## Practical Exercises

- Buy the systems design book and look into reading list: https://blog.pragmaticengineer.com/my-reading-list/

## Questions

- How do websockets differ from HTTP?
