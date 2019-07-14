




## CloudFormation

CloudFormation takes JSON/YAML config and calls AWS API to create resources.
Save time configuring automation manually
Can be used to rollback or delete an entire stack
Optional fields
Description — Overview of your stack
Metadata — Data about your stack
Parameters — Values to pass into a template (can be validated with “allowed values”)
Conditions — Allows you to wrap resource creation based on conditions
Transform — Can reference snippets code from outside of the template (i.e S3)
