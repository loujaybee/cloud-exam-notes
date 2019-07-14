
# The 5 Pillars Overview

The well architected framework is based on five pillars:

- Operational Excellence
- Security
- Cost Optimisation
- Reliability
- Performance Efficiency

## Game Days

- Setting up and testing your response processes
- Standing up your prod architecture and attacking it for the purposes of breaking it

## Pillar 1: Operational Excellence

_Does our application work? Will our application continue to work?_

* Operations as code
* Documentation is updated automatically
* Small changes (with rollback plans)
* Tighten feedback loops (iterate)
* Expect failure (red team, game days)
* Learn from failures and successes

## Pillar 2: Security

_Does it do what we want? (And only that?)_

* Identities have the least priveledges possible
* Who who did what and when?
* Automate security tasks
* Encrypt data at transit and at rest
* Prepare for the worst (woven into game days)

## Pillar 3: Cost Optimisation

_Spend only what you have to_

* Consumption based pricing
* Measure efficiency constantly
* Let AWS do the work when necessary
* Ties closely to operational performance

## Pillar 4: Reliability

_Is what you built going to work consistently?_

* Recover from issues automatically
* Scale horizontally (where possible)
* Reduce idle resources (operational burden, attack surface increase)
* Manage change through automation

## Pillar 5: Performance Efficiency

* Let AWS do the work (when possible)
* Reduce latency through regions & AWS edge
* Serverless > Containers > Instances
