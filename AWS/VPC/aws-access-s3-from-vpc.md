

# Access S3 from a VPC

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-s3)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated virtual network on the cloud. It is useful because companies need to manage resources according to their own rules and security requirements.

### What I built

I launched an EC2 instance in the public subnet of a VPC, configured AWS CLI credentials on it, and used it to interact with an S3 bucket — listing buckets, listing objects, and uploading files directly from the instance.

### One thing I didn't expect

One thing I didn't expect in this project was how straightforward the setup was once the VPC and subnet were in place.

### Time to complete

This project took me 45 minutes.

---

## Architecture

### Step 1 — Architecture set up

I started by creating a VPC and launching an EC2 instance in a public subnet. I also created an S3 bucket and uploaded some images to it so I could access them later from the instance.

![Architecture Diagram](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-s3_4334d777)

---

## Connecting to the EC2 Instance

### Step 2 — Connect to the EC2 instance

After launching the instance, I connected to it directly via EC2 Instance Connect. From inside the instance, I had access to AWS CLI to interact with AWS services.

### Step 3 — Set up access keys

To give the EC2 instance permission to access S3, I configured AWS CLI using the `aws configure` command. This required:

- **Access Key ID** — acts as the username for programmatic access
- **Secret Access Key** — acts as the password paired with the access key ID
- **Default region** — the AWS region to target
- **Output format** — how CLI responses are displayed

#### Security best practice

Although I used access keys in this project, the recommended alternative is to attach an **IAM role** to the EC2 instance. IAM roles provide temporary, scoped credentials without storing long-term secrets on the instance.

![AWS Configure](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-s3_e7fa8776)

---

## Accessing S3 from the EC2 Instance

### Step 4 — List S3 buckets

The first command I ran was `aws s3 ls`, which lists all S3 buckets in my account. The terminal returned the name of my bucket, confirming the credentials were working.

![S3 Bucket List](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-s3_4334d778)

### Step 5 — List objects in the bucket

Next, I ran `aws s3 ls s3://<my-bucket-name>` to list all the files inside the bucket. This confirmed that the EC2 instance could reach and read from S3.

![S3 Object List](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-s3_4334d779)

### Step 6 — Upload a file to S3

To test write access, I:

1. Created a new file on the instance using the `touch` command
2. Uploaded it with `aws s3 cp <file-name> s3://<bucket-name>`
3. Verified the upload with `aws s3 ls s3://<bucket-name>`

The file appeared in the bucket, confirming full read/write access from the EC2 instance.

![File Upload](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-s3_3e1e79a2)

---
