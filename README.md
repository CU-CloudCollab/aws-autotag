# aws-autotag
Example CloudFormation template to create a lambda function that automatically tags EC2 resources when they are created.

## Details

This example is derived from [this AWS blog post](https://blogs.aws.amazon.com/security/post/Tx150Z810KS4ZEC/How-to-Automatically-Tag-Amazon-EC2-Resources-in-Response-to-API-Events).

This AWS CloudFormation template creates a Lambda function and necessary infrastructure (e.g., policies, rules) to automatically tag EC2 resources when they are created. The example tags EC2 instances, volumes, network interfaces, AMIs, and Snapshots with "Creator" and "PrincipalId" tags. The "Creator" value is derived from the IAM user name, or federated identity principal.

## Resources Created

* the Lambda function itself
  * An explicit "stable" version of the function
  * "PROD" alias to the "stable" version.
* an IAM role for the lambda function to assume when executing (which allows the function to create tags and describe EC2 resources).
* a CloudWatch rule to trigger the Lambda function when an EC2 resource is created. I.e., upon any of the following events:
  * CreateVolume
  * RunInstances
  * CreateImage
  * CreateSnapshot
* Permission for the rule to invoke the lambda function

## Testing

Once the Lambda function is created by the CloudFormation template, you can manually configure the function to use the included test event [test-event.json](https://github.com/CU-CloudCollab/aws-autotag/blob/master/test-event.json) to test the Lambda function in the AWS console.

## Known Issues

* Presently, this function misses tagging snapshots created when an image is created. It tags the image itself, but not the snapshots underlying the image because those snapshot creations are not first class events. To fix this, the Lambda function probably would need to pick apart the sub resource (volumes) that makeup the snapshot during the CreateSnapshot event.
