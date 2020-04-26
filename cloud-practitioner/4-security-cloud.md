
# Security In The Cloud

// TODO: 👷‍♀ aws.amazon.com/compliance

## 1. Security Concepts

#### Share Responsibility Model

**AWS:**
- Hardware, Compute, Databases, Networking

**Customer**
- Customer data
- Firewall configuration
- Networking traffic

// TODO: 👷‍♀ https://aws.amazon.com/compliance/shared-responsibility-model/

## 2. Security AWS Services

#### AWS Artifact (Audit?)
- A comprehensive list of compliance documents

#### AWS Config
- Monitors your server configurations (security groups, etc)

#### CloudWatch
- Application Performance Monitoring

#### CloudTrail
- Setup one S3 in an account and route all CloudTrail logs here
- API Calls (not application monitoring)

#### AWS WAF
- WAF (Layer 7, Application Firewall) prevents XSS and SQL Injection

// TODO: 👷‍♀ Investigate more about this service

#### AWS Shield
- DDoS Mitigation
- Turned on default, advanced is $3000 a month
- Two flavours: Standard + Advanced
- Advanced
    - DDos Response Team (and post-attack analysis)
    - Cost protection (don't have to pay during an attack)

// TODO: 👷‍♀ Investigate more about this service

#### AWS Inspector
- Agent installed on EC2 Instances (Automated assessment)

#### AWS Trusted Advisor
- Online Service for providing insight
- Helps with reducing cost and performance (not just security)
- Need to update support plan for all checks (cost optimisation)

### Security As Code
- Hardened EC2's
