
# Host a Website on Amazon S3

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-host-a-website-on-s3)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Project Overview

In this project, I hosted a static website on AWS S3 using HTML and image files. I configured the bucket for public access using static website hosting, ACLs, and bucket policies.

**Services used:** Amazon S3, ACLs, Bucket Policies  
**Duration:** ~25 minutes

---

## Step 1 — Create an S3 Bucket

I logged into the AWS Console and navigated to S3 to create a new bucket.

- **Region:** Mumbai (`ap-south-1`) — closest to my location
- **Time to create:** ~2 minutes

**Key concept:** S3 bucket names are globally unique. Once a bucket is created, no other AWS account in the world can use that name until the bucket is deleted.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_ba6d42ad)

---

## Step 2 — Upload Website Files

I uploaded the following files to the S3 bucket:

- **`index.html`** — the entry point and structure of the website
- **Image assets** — displayed on the webpage

Both files are required: `index.html` provides the skeleton, and the images populate the page content.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_a265af88)

---

## Step 3 — Enable Static Website Hosting

To make the bucket serve a website, I navigated to:  
**Bucket → Properties → Static Website Hosting → Edit → Enable**

**What website hosting does:** It exposes your S3 bucket files via a public URL, making the site accessible on the internet.

I also enabled **ACLs (Access Control Lists)** — a set of rules that control who can access individual objects in the bucket.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_c22c54c0)

---

## Step 4 — Test the Bucket Endpoint

After enabling static website hosting, S3 generates a **bucket website endpoint URL** — a public link to access the hosted site.

When I first visited the endpoint, I got a **403 Forbidden** error. This meant the bucket was configured for hosting, but the uploaded files were still private by default.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_22ce4daf)

---

## Step 5 — Make Files Public

To resolve the 403 error, I selected `index.html` and the assets folder, then chose:  
**Actions → Make Public using ACL**

After applying this, the website loaded successfully at the bucket endpoint URL.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Extension — Bucket Policies

As an extension, I configured a **bucket policy** to manage access at a more granular level.

**Bucket policies vs. ACLs:** ACLs control read/write access per object. Bucket policies go further — they can control who can delete objects, upload new files, change permissions, and much more, all from a single JSON policy document.

I tested the policy by attempting to delete `index.html`, which was blocked as expected — confirming the policy was enforced correctly.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_sm2sm2sm)
