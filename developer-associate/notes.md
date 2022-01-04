

## Courses

- ExamPro: https://app.exampro.co/student/material/dva-c01/1188
- Stephane Mareek: https://www.udemy.com/course/aws-certified-developer-associate-dva-c01/learn/lecture/19733666?start=15#overview

## AWS Step Functions

- A state machine for serverless workflows
- States are configured through Amazon States Language (JSON)
- Standard vs express workflows
- States:
    - **Pass state** - Which passes input through to output.
    - **Task state** - Uses a Lambda, or passing parameters to other AWS services.
    - **Activity state** - Work is offloaded to a worker, e.g. EC2, ECS, etc. 
    - **Parallel state** - 
- Seems very coupled to AWS, I wonder about how we can keep this agnostic

## Questions

- SF: What's the difference betweens standard and express workflows in step functions?
- SF: How are the other services invoked via the parameters? How does that work in more advanced cases?
- SF: When calling out to another service, how does it call back? Is that managed via the SDK? Or via something else?

## Hands On

- SF: Create your own step-functions example project