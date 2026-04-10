
# Host a Website on Amazon S3

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-host-a-website-on-s3)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Introducing Today's Project!

### Project overview

In this project, I will be using AWS S3 Bucket to host a website using files and pictures.

### Tools and concepts

Services I used were S3 Bucket, ACLs, Bucket policies to live my website publicly and manage access to it.

### Time, challenges, and wins

This project took me approximately 25 minutes.

---

## How I Set Up an S3 Bucket

### What I did in this step

In this step, I will log into my AWS console and search for S3.

### How long it took to create the bucket

Creating an S3 bucket took me around 2 minutes

### Region selection

The Region I picked for my S3 bucket was Mumbai(ap-south-1) because its closest to me.

### Understanding bucket name uniqueness

An S3 bucket name is globally unique. After you create a bucket, no other AWS account in the entire world can use your bucket's name (unless you delete the bucket).

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_ba6d42ad)

---

## Upload Website Files to S3

### What I did in this step

In this step, I will uploading the HTML file that sets upthe website and images for my website.

### Files I uploaded

I uploaded two files to my S3 bucket - they were index.html the entry point and the images for my website.

### How the files work together

Both files are necessary for this project as index.html is just the skeleton and entry point, and images will be shown on this page.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_a265af88)

---

## Static Website Hosting on S3

### What I did in this step

In this step, I will configure my S3 bucket for static website hosting and visit public website link.

### Understanding website hosting

Website hosting is what makes your website public on the internet.

### How I enabled website hosting

To enable website hosting with my S3 bucket, I selected the targeted bucket -> properties -> all the way down to static web hosting -> Edit -> Enable it

### Access Control Lists (ACLs)

An ACL is a set of rules that decides who can get access to a resource.
I enabled it.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_c22c54c0)

---

## Bucket Endpoints

### Understanding bucket endpoint URLs

A bucket website endpoint is just like a regular website URL. It lets people visit your S3 bucket's files as a website.

### What I saw when I tested the endpoint

When I first visited the bucket endpoint URL, I saw an error 403. The error message is telling me that your static website is being hosted by S3, but the actual HTML/image files I have uploaded are still private.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_22ce4daf)

---

## Success!

### What I did in this step

Files uploading done, now I will make website files in S3 publicly accessbile and then verify if its live.

### How I resolved the 403 error

To resolve this 403 Forbidden error, I selected the index.html and assests foler -> Actions -> Make Public using ACLs

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_5d4474f9)

---

## Bucket Policies

### What I did in this extension

In this project extension I'm about to set up bucket policies so I can manage control access to index.html file of my website.

### Understanding bucket policies

An alternative to ACLs are bucket policies.
Using bucket policies means you can now control more than just who can see/access an object. With bucket policies, you can manage:
Who can delete the object.
Who can change the object.
Who can upload new objects to your bucket.
And much, much more!

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-host-a-website-on-s3_sm2sm2sm)

### What my bucket policy does

I tested the bucket policy by trying to delete index.html. 

