
# Storage & Data Management

## Snowball / Snowball Edge
* Used for moving on-premise data into the cloud (bypassing the internet)
* If data takes more than a few days or a week to upload, you should use snowball (cheaper, easier)
* Used for large-scale data transfer
* Snowball edge is used for analysis before you upload to S3

## AMI's
* Are a snapshot of your EC2
* Cannot snapshot certain images (with licenses like Oracle)
* Encrypted AMI's cannot be copied, they must be un-encrypted, moved and then re-encrypted.

## Athena
* Querying from inside of S3
* Use a SQL like langauge
* Is a serverless cloud solution (pay per query)
* Can query cloudwatch, cloudtrail, S3 Access, Website Logs (S3)

## S3
* Appears in the exam quite a lot
* Allows you to configure lifecycle policies
* Infrequently accessed and glacier options (cheaper than regular S3)
* Can schedule files to be deleted after a certain amount of time
* Can enable MFA delete to protect delete of S3 resources
* Encryption in transit (SSL / TLS)

## Instance Store
* Data is ephemeral

## KMS
* KMS and CloudHSM generate, store and manage secrets / keys.
* CloudHSM allows for dedicated hardware for generating specific keys.

## Questions
* What are the key differences between snowball and snowball edge?
* Difference between SSL and TLS?
* What's the difference between encryption types in S3?
* What is instance store?
* Is EBS guarenteed persistence?
* S3 as NFS vs EBS (what's the cost/benefits)
* What is CloudHSM
* What is Storage Gateway?
