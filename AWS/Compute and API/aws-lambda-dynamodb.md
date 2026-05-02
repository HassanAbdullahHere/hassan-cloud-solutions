
# Fetch Data with AWS Lambda

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-lambda)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Overview

In this project, I created a DynamoDB table to store user data, then built a Lambda function to retrieve that data on demand. I validated the function using test events, diagnosed a permissions error from the execution logs, and applied the correct IAM policy to fix it.

**Services used:** AWS Lambda, Amazon DynamoDB, IAM  
**Duration:** ~50 minutes

---

## Project Setup

I started by creating a DynamoDB table with `userId` as the partition key, so each item can be uniquely identified.

I then added a test user item with an `id`, `name`, and `email` attribute.

![DynamoDB table setup](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-lambda_a112c3d5)

---

## What is AWS Lambda?

AWS Lambda is a serverless compute service — it runs your code in response to events without requiring you to provision or manage servers. You only pay for the compute time your function actually uses.

---

## Lambda Function

I created a Lambda function with an **execution role** — an IAM role that controls which AWS services the function is allowed to access.

The function uses the **AWS SDK for JavaScript** to interact with DynamoDB. It accepts a `userId` as input, queries the `UserData` table for a matching item, and returns the result. A `try/catch` block handles failures and surfaces a descriptive error message when something goes wrong.

![Lambda function code](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-lambda_a1b2c3d5)

---

## Function Testing

I created a test event with the following payload to verify the function:

```json
{
  "userId": "1"
}
```

> **Note:** A "Success" execution status means the function ran without code errors — it does not guarantee the function retrieved the expected data. Always check the response body.

![Lambda test result](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-lambda_u1v2w3x4)

---

## Fixing Permissions

The initial test returned an `AccessDeniedException`. The error message identified the exact missing permission: `dynamodb:GetItem`.

I reviewed four DynamoDB managed policies and selected **AmazonDynamoDBReadOnlyAccess** because it includes `dynamodb:GetItem` and grants only read access — following the principle of least privilege.

![IAM permissions update](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-lambda_3ethryj2)

---

## Final Validation

After attaching the policy, I re-ran the test event and confirmed the function successfully returned the user record from DynamoDB.

### Real-World Use Cases

Lambda + DynamoDB is a common pattern in production applications:

- **E-commerce** — fetch product details or a customer's order history
- **Social media** — retrieve user profiles or linked media content
- **Publishing** — serve articles or blog posts based on user queries

![Final test success](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-lambda_p9thryj2)

---

## Enhancing Security

Future improvements to harden this setup:

- **Scope down the IAM policy** — create a custom policy that restricts `dynamodb:GetItem` to only the specific `UserData` table ARN instead of all DynamoDB resources.
- **Enable encryption at rest** — use AWS KMS to encrypt the DynamoDB table.
- **Add input validation** — sanitize the `userId` input inside the Lambda function before passing it to DynamoDB.
- **Enable Lambda logging** — send execution logs to CloudWatch for auditing and monitoring.

---
