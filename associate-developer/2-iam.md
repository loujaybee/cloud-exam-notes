
## Regions, AZ's and IAM

## AWS Regions

- Geographical regions
- Each region can have multiple availability zones (AZ)
- Each region has differing number of AZ's
- AZ's are geographically isolated for disasters
- Useful to understand [Global infrastructure](https://aws.amazon.com/about-aws/global-infrastructure)

## IAM

- IAM has managed policies
- IAM is a global service (i.e not created per region)
- Create root account once, then don't use it again
- Users, groups and roles, policies
- IAM Federation
    - Integrating users from external sources
    - Allows you to login to AWS using company credentials
    - Identity federation uses the SAML standard (Active Directory is a SAML example)

## IAM Best Practices

- 1 IAM user per person
- 1 IAM role per application
- IAM credentials should NEVER BE SHARED
- Do not write IAM credentials into code (ever)
- Don't use root account (apart from initial setup)

## Practical Exercises

- Look into SAML and how it works
- Setup account with new users (not root)
