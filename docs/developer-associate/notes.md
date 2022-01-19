
## Overview
- **Days to go**: 16
- **Percentage remaining**: 94%

## Todo
- Run through: 
  - Networking services
  - The "DevOps" services
  - Core services (Lambda, EC2, RDS, etc)
- Update your imaginary business repo with some new services
  - Create an AWS diagram
- Check out the other courses to see if anything is missing
- Buy prep questions

## Courses

- ExamPro: https://app.exampro.co/student/material/dva-c01/1188
- Stephane Mareek: https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/learn/lecture/19733666?start=15#overview

## Concepts

### CI/CD/CD
- Continuous Integration: Code, Build, Integrate, Test
- Continuous Delivery: Code, Build, Integrate Test, Release (preparing code for deployment, but deployment is still a manual process)
- Continuous Deployment: Code, Build, Integrate Test, Release, Deployment 

## Services

### AWS SAM
- Extension of CloudFormation (with CLI)
- Also a CloudFormation Macro (a DSL)

### Parameter Store (AWS Systems Manager)
- Store data like passwords, database
- Create hierarchies like directories (using forward slashes)
- Can encrypt the string with KMS
- Free up to 10,000 parameter fetches
- Pricing: Standard and Advanced (parameter policies)
- Parameter policies allow you to emit triggers e.g. notifications on parameter changes
- Parameter policies: Expiration, NoChangeNotification, ExpirationNotification
- Everything in parameter store is automatically versioned
- You can't revert an advanced parameter due to it's size

### Amazon Cognito
- **Cognito User Pools** - User directories that provide sign-up and sign-in options for your apps users. Manage actions like sign-up, sign-in, account recovery, confirmation. Works via JWT. Can integrate events with Lambda. 
- **Cognito Identity Pools** - Grant temporary access to other AWS services.
- **Cognito Sync** - Syncs user data and preferences across multiple devices (uses SNS under the hood). Is key-value data. 
- Amplify CLI provisions cognito under the hood 

### AWS CloudFormation
- Macro's are DSL's that provide extensions to CloudFormation. Macro's are created via AWS Lambda. 
- 

### AWS Step Functions
- A state machine for serverless workflows
- States are configured through Amazon States Language (JSON)
- Standard vs express workflows
- States:
    - **Pass state** - Which passes input through to output.
    - **Task state** - Uses a Lambda, or passing parameters to other AWS services.
    - **Activity state** - Work is offloaded to a worker, e.g. EC2, ECS, etc. 
    - **Parallel state** - 
- Seems very coupled to AWS, I wonder about how we can keep this agnostic

### Code Build
- Works via a `buildspec.yml` declarative build file (but you can override with the CLI)
- Buildspec versions: `0.1` runs each command in a separate instance of the default shell, `0.2` is recommended
- Two options for providing the build specifications: build commands, the buildspec file.
- There are some managed images to use out-of-the-box
- Supports the following: `CodeCommit`, `S3`, `GitHub`, `Bitbucket`.
- `buildspec.yml` needs to be at the root of your directory and exactly named
- **Phases:** `install`, `pre_build`, `build`, `post_build`

### Code Deploy
- Fully managed service in the cloud
- Configured using the `appspec.yml` file
- Can manage traffic shifting, e.g. doing canary, blue/green type deployments, and in-place deployments
- Provide bash scripts for all the hooks (app stop, before install, after install), which are zipped up
- Hooks are different depending if you're using lambda, ec2, ecs, you can see progress / result of hooks, but it won't show you the logs from EC2, so you ideally should hook it up to CloudWatch
- Can deploy on EC2, on-premise, lambda or ECS
- Update AWS Lambda function versions
- Components: **Deployment Groups** (resources to deploy to), **Deployment** (apply a revision), **Configuration** (what to do during deployment e.g. success/failure), **AppSpec file** (server definition), revision (everything required to deploy a new version).

### Code Pipeline
- Continuous Delivery service (integrates CodeCommit, CodeBuild and CodeDeploy)
- A **pipeline** encompasses all components, a **stage** is a step in the pipeline, **action** does something, e.g. pull code, run tests, deploy to prod, **artifact** is created (optionally) for each **action**.
- Many **actions** integrate with AWS, but you can also create custom actions

### Code Star
- Wizard for getting common application projects set up
- Has project management, issue tracking
- Three components of CodeStar: Project dashboard, deployment pipeline, access management

## Questions
- COG: How does SAML really work?
- COG: Difference between cognito user pools vs AWS log in via SSO
- COG: What's the difference between cognito and Auth0?
- PS: Can parameter store be used for storing passwords? Why?
- CD: What's the difference between CodePipeline and Code Deploy?
- CD: Why over GitHub Actions, for instance?
- CB: How does the parameter store integration work?
- CB: Why is Code Build separate to Code Deploy?
- SF: What's the difference betweens standard and express workflows in step functions?
- SF: How are the other services invoked via the parameters? How does that work in more advanced cases?
- SF: When calling out to another service, how does it call back? Is that managed via the SDK? Or via something else?

## Hands On
- Cognito: Authenticate the user
- CD: Use CodeDeploy to deploy EC2 instances
- CB: Run a code build pipeline
- SF: Create your own step-functions example project

## Further Reading
- [Complete CI/CD with AWS CodeCommit, AWS CodeBuild, AWS CodeDeploy, and AWS CodePipeline](https://aws.amazon.com/blogs/devops/complete-ci-cd-with-aws-codecommit-aws-codebuild-aws-codedeploy-and-aws-codepipeline/)
- [The Case For And Against Cognito](https://theburningmonk.com/2021/03/the-case-for-and-against-amazon-cognito/)
- [CloudFormation Macro's](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html)