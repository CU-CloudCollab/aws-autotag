# aws-autotag
Example CloudFormation template to create a lambda function that automatically tags EC2 resources when they are created

## Details

This example is derived from [this AWS blog post](https://blogs.aws.amazon.com/security/post/Tx150Z810KS4ZEC/How-to-Automatically-Tag-Amazon-EC2-Resources-in-Response-to-API-Events).

This AWS CloudFormation template creates a Lambda function and necessary infrastructure (e.g., policies, rules) to automatically tag EC2 resources when they are created. The example tags EC2 instances, volumes, network interfaces, AMIs, and Snapshots with "Creator" and "PrincipalId" tags. The "Creator" value is derived from the IAM user name, or federated identity principal.

## Resources Created

* Lambda function itself
  * An explicit "stable" version of the function
  * "PROD" alias to the "stable" version.
* IAM role for the lambda function to assume when executing (which allows the function to create tags and describe EC2 resources).
* CloudWatch rule to trigger the Lambda function when an EC2 resource is created. I.e., upon any of the following events:
  * CreateVolume
  * RunInstances
  * CreateImate
  * CreateSnapshot
* Permission for the rule to invoke the lambda function
