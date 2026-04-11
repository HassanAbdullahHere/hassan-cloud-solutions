
# Cloud Security with AWS IAM

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-security-iam)  
**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Project Banner](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_1c864649)

---

## Project Overview

In this project, I used the AWS Identity and Access Management (IAM) service to control who is **authenticated** (signed in) and **authorized** (has permissions) in my AWS account.

### Tools & Concepts

| Service / Concept    | Purpose                                      |
|----------------------|----------------------------------------------|
| IAM Policies         | Define what actions are allowed or denied    |
| IAM Users            | Represent individual identities in AWS       |
| IAM User Groups      | Manage permissions for multiple users at once|
| AWS Account Alias    | Provide a friendly login URL for the account |

**Estimated time:** ~50 minutes

---

## Step 1 — EC2 Instance Tags

### What I Did

I created two EC2 instances and applied tags to differentiate between **production** and **development** environments.

### Understanding Tags

Tags are key-value labels attached to AWS resources. They help organize, identify, and apply conditional policies to resources based on their purpose or environment.

### My Tag Configuration

I applied an `Env` (Environment) tag to both EC2 instances:

| Instance    | Tag Key | Tag Value     |
|-------------|---------|---------------|
| Production  | `Env`   | `production`  |
| Development | `Env`   | `development` |

![Tag Configuration](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_2e0e5a5d)

---

## Step 2 — IAM Policies

### What I Did

I created an IAM policy in JSON to grant an intern access to the **development** EC2 instance only.

### Understanding IAM Policies

An IAM policy is a set of rules that defines what actions are allowed or denied on specific AWS resources. Policies can be attached to users, groups, or roles.

### Key Policy Components

| Component  | Description                                                |
|------------|------------------------------------------------------------|
| `Effect`   | Either `Allow` or `Deny` — indicates the policy outcome    |
| `Action`   | The list of AWS actions the policy permits or restricts    |
| `Resource` | The specific AWS resources the policy applies to           |

### Policy Behavior

This policy:
- **Allows** actions such as `StartInstances`, `StopInstances`, and `DescribeInstances` — but only for instances tagged `Env = development`
- **Denies** the ability to create or delete tags on **all** instances

### JSON Policy

![IAM Policy JSON](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_1c864649)

---

## Step 3 — Account Alias

### What I Did

I created an AWS Account Alias to simplify the sign-in URL for the intern.

### Understanding Account Aliases

An Account Alias is a human-readable name for your AWS account. Instead of signing in with a long numeric account ID, users can use a custom alias URL like:

```
https://<alias>.signin.aws.amazon.com/console
```

### Setup

Creating the alias took approximately 2 minutes.

![Account Alias](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_0eb4439b)

---

## Step 4 — IAM Users and User Groups

### What I Did

I created a dedicated IAM user group for all interns and added a new IAM user to that group.

### Understanding User Groups

An IAM User Group is a collection of IAM users. By attaching a policy to the group, every user in the group inherits the same permissions — making it easy to manage access at scale.

### Understanding IAM Users

IAM users represent individual people or services that need access to your AWS account. Assigning users to groups (instead of attaching policies directly) keeps permission management clean and scalable.

### What I Set Up

- Created a group: **Interns**
- Attached the IAM policy (created in Step 2) to the group
- Created an IAM user for the new intern and added them to the **Interns** group

---

## Step 5 — Logging in as an IAM User

### Sharing Sign-In Details

AWS provides two ways to share access credentials:

1. **AWS Management Console** — share the alias login URL, username, and a temporary password
2. **Programmatic access** — share access keys for use with the AWS CLI, SDKs, or APIs

### Observations from the IAM User Dashboard

After logging in as the intern IAM user, several dashboard panels displayed **Access Denied** — confirming the policy is correctly restricting access to unauthorized services.

![IAM User Dashboard](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_6f2ab446)

---

## Step 6 — Testing IAM Policies

### What I Did

I tested the intern's access permissions by attempting to stop both the production and development EC2 instances.

### Test Results

| Action                         | Instance    | Result  |
|--------------------------------|-------------|---------|
| Stop instance                  | Production  | Denied  |
| Stop instance                  | Development | Allowed |

### Stopping the Production Instance

When attempting to stop the production instance using the intern login, AWS returned:

> "Failed to stop the instance."

This confirms the policy correctly **denies** access to production resources.

![Production Stop — Denied](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_0e7a9d6a)

### Stopping the Development Instance

When attempting to stop the development instance, the action succeeded and the instance status changed to **Stopped**.

![Development Stop — Success](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_1811801c)

---

## Step 7 — IAM Policy Simulator

### What I Did

I used the AWS IAM Policy Simulator to test the policy without needing to switch accounts.

### Understanding the IAM Policy Simulator

The IAM Policy Simulator lets you test the effect of IAM policies directly in the console. It's useful for validating policies before assigning them to real users, avoiding the need to log in and out of multiple accounts.

### Simulation Results

| Action           | Condition              | Result  |
|------------------|------------------------|---------|
| Remove tags      | No condition           | Denied  |
| Stop instance    | No `Env` value set     | Denied  |
| Stop instance    | `Env = development`    | Allowed |

Setting the `Env` context key to `development` was required for the stop action to pass — confirming the policy condition is working as expected.

![Policy Simulator](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-security-iam_069d8a621)

---
