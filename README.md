# chrisyeo-assign2-12
Function as a service
Given a Lambda function that is triggered upon the creation of files in an S3 bucket, answer the following:
**What is the purpose of the execution role on the Lambda function?**

## **Permissions for Lambda to Access AWS Services**: The Lambda execution role is an IAM role that grants the Lambda function permissions to access other AWS services. When your Lambda function runs, it assumes this role.
**Essential for Function Operation**: Without the proper execution role, your Lambda function won't be able to interact with any AWS services beyond its own runtime environment. This includes things like:
1. Writing logs to CloudWatch Logs.
2. Reading or writing to S3 buckets (other than the triggering bucket).
3. Accessing DynamoDB, SNS, etc.

## **What is the purpose of the resource-based policy on the Lambda function?**
**Permissions for AWS Services to Invoke the Lambda Function**: The resource-based policy (also known as a Lambda function policy) grants permissions to other AWS services (like S3) to invoke your Lambda function.
**Triggering Events**: In your case, this policy allows S3 to trigger the Lambda function when a file is created.
**Specific Invocation Control**: It provides fine-grained control over which AWS services and resources are allowed to invoke the function.

## **If the function is needed to upload a file into an S3 bucket, describe (i.e no need for the actual policies)**
**Execution Role Update**:
The execution role needs to be updated to include permissions allowing the Lambda function to perform s3:PutObject actions on the target S3 bucket.
The execution role should follow the principle of least privilege, meaning it should only have the necessary permissions.
**New Resource-Based Policy (if any)**:
In this scenario, where the Lambda function is triggered by S3 and then uploads to another or the same S3 bucket, it is very unlikely that you will need to add or modify the resource based policy. The resource based policy is for the S3 service to invoke the Lambda function. The Lambda function invoking S3 requires the execution role. Therefore, no changes to the resource based policy are likely needed.

## **What is the needed update on the execution role**?
To allow the Lambda function to upload a file to an S3 bucket, the execution role needs to include a policy that grants the s3:PutObject permission

## **What is the new resource-based policy that needs to be added (if any)**?
 No new resource-based policy needs to be added
 Okay, let's reiterate to be absolutely clear:

In the scenario where an S3 event triggers a Lambda function, and that Lambda function then uploads a file to an S3 bucket, the following is true regarding the resource based policy.

The S3 trigger already establishes the necessary resource-based policy. When you configure S3 to trigger your Lambda function, AWS automatically adds a resource-based policy to the Lambda function. This policy grants S3 the permission to invoke the Lambda function.
No additional resource-based policy is typically needed for the Lambda function to upload to S3. The Lambda function's ability to upload to S3 is governed by its execution role, not the resource-based policy.
