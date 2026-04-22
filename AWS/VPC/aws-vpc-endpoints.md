
# VPC Endpoints

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-endpoints)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated virtual network on the cloud used to manage and secure AWS resources.

### What I built

I extended a standard VPC + EC2 + S3 setup by creating a **VPC Gateway Endpoint** for S3, which routes traffic between the EC2 instance and S3 privately — without going through the public internet. I then locked down the S3 bucket with a bucket policy that denies all access except through the endpoint, and tested endpoint-level access control using an endpoint policy.

### One thing I didn't expect

One thing I didn't expect was how much bucket policies and endpoint policies interact — applying one without updating the route table breaks access in a non-obvious way.

### Time to complete

This project took me 60 minutes.

---

## Part 1 — Baseline Setup

### Step 1 — Architecture set up

I started by launching an EC2 instance in the public subnet of a VPC, and creating an S3 bucket with two image files uploaded to it.

![Architecture Diagram](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_4334d777)

### Step 2 — Connect to the EC2 instance

I connected to the EC2 instance directly via EC2 Instance Connect to get terminal access.

### Step 3 — Set up access keys

To allow the EC2 instance to interact with AWS services via CLI, I ran `aws configure` and entered:

- **Access Key ID** — the identifier for programmatic access
- **Secret Access Key** — the credential paired with the key ID
- **Default region** — the AWS region to target
- **Output format** — how CLI responses are displayed

Access keys act like a username and password for the AWS CLI. The secret access key must be kept private.

#### Security best practice

Although I used access keys here, the recommended alternative is to attach an **IAM role** to the EC2 instance. Roles provide temporary, scoped credentials and avoid storing long-term secrets on the instance.

### Step 4 — Verify S3 access

To confirm the credentials were working, I ran two commands:

- `aws s3 ls` — listed all S3 buckets in my account
- `aws s3 ls s3://<bucket-name>` — listed the files inside my specific bucket

Both returned the expected output, confirming the EC2 instance had access to S3 over the internet.

![Bucket List](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_4334d778)

![Object List](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_4334d779)

![Uploaded Objects](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_3e1e79a2)

---

## Part 2 — VPC Endpoint Setup

### Step 5 — Create a Gateway Endpoint

A **VPC Endpoint** allows private connections between your VPC and AWS services without routing traffic over the public internet. A **Gateway Endpoint** is the specific type used for S3 and DynamoDB.

I created a Gateway Endpoint targeting S3 and associated it with my VPC.

![Gateway Endpoint](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_09bcaa8a)

### Step 6 — Apply a bucket policy

A bucket policy is a resource-based IAM policy that controls who can access an S3 bucket and what actions they can perform.

I configured my bucket policy to **deny all access except requests coming through the VPC Endpoint**. Immediately after saving, the S3 console showed "Access Denied" warnings — expected, because access is now restricted to the endpoint path only.

![Bucket Policy](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_7316a13d)

![Access Denied Warning](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_4ec7821f)

### Step 7 — Update the route table

The EC2 instance still couldn't reach S3 through the endpoint because the public subnet's route table had no route pointing to it. I fixed this by updating the endpoint configuration to reference the correct route table for my public subnet.

After the update, `aws s3 ls s3://<bucket-name>` returned the file list again — traffic was now flowing through the endpoint.

![Route Table Update](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_d116818e)

### Step 8 — Test endpoint policy

An endpoint policy controls who can use the endpoint itself — an additional layer of access control on top of the bucket policy.

I updated the endpoint policy to set `Effect` to `Deny`, which immediately blocked the EC2 instance from accessing S3 through the endpoint, confirming that endpoint policies enforce access at the network level.

![Endpoint Policy](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-endpoints_3e1e79a3)

---
