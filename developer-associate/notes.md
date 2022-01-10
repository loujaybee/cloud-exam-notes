

## Courses

- ExamPro: https://app.exampro.co/student/material/dva-c01/1188
- Stephane Mareek: https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/learn/lecture/19733666?start=15#overview

## Concepts

### CI/CD/CD

- Continuous Integration: Code, Build, Integrate, Test
- Continuous Delivery: Code, Build, Integrate Test, Release
- Continuous Deployment: Code, Build, Integrate Test, Release, Deployment

## Services

### AWS SAM

- Extension of CloudFormation (with CLI)
- Also a CloudFormation Macro (a DSL)

### AWS CloudFormation

- Macro's are DSL's that provide extensions to CloudFormation. Macro's are created via AWS Lambda. 

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
- There are some managed images to use out-of-the-box
- Supports the following: `CodeCommit`, `S3`, `GitHub`, `Bitbucket`.
- `buildspec.yml` needs to be at the root of your directory and exactly named
- **Phases:** `install`, `pre_build`, `build`, `post_build`

### Code Deploy

- Can manage traffic shifting, e.g. doing canary, blue/green type deployments, and in-place deployments
- Provide bash scripts for all the hooks (app stop, before install, after install), which are zipped up
- Hooks are different depending if you're using lambda, ec2, ecs, you can see progress / result of hooks, but it won't show you the logs from EC2, so you ideally should hook it up to CloudWatch
- Can deploy on EC2, on-premise, lambda or ECS
- Update AWS Lambda function versions
- Components: **Deployment Groups** (resources to deploy to), **Deployment** (apply a revision), **Configuration** (what to do during deployment e.g. success/failure), **AppSpec file** (server definition), revision (everything required to deploy a new version).

## Questions

- CD: Why over GitHub Actions, for instance?
- CB: How does the parameter store integration work?
- CB: Why is Code Build separate to Code Deploy?
- SF: What's the difference betweens standard and express workflows in step functions?
- SF: How are the other services invoked via the parameters? How does that work in more advanced cases?
- SF: When calling out to another service, how does it call back? Is that managed via the SDK? Or via something else?

## Hands On

- CD: Use CodeDeploy to deploy EC2 instances
- CB: Run a code build pipeline
- SF: Create your own step-functions example project