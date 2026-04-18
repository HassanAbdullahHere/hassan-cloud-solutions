
# Launching VPC Resources

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-ec2)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Overview

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-ec2_8ee57662)

---

## Introducing Today's Project

### What is Amazon VPC?

Amazon VPC is an isolated space on the cloud to manage personal resources.

### How I used Amazon VPC in this project

I used Amazon VPC to create private and public subnets for resources.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was the VPC wizard, which is actually a fantastic tool.

### This project took me...

This project took me 50 minutes.

---

## Setting Up Direct VM Access

Directly accessing a virtual machine means logging into and managing the operating system or software of the machine as if you were using it in front of you, but over the internet.

### SSH is a key method for directly accessing a VM

SSH, or Secure Shell, is the protocol we use for secure access to a remote machine.

### To enable direct access, I set up key pairs

Key pairs help engineers directly access their virtual machines, like EC2 instances.

Just like how documents can be saved in various file formats like PDF, DOCX, or TXT — each suited for different applications or systems — private keys also come in different file formats. Not every system or application can process all these formats, so choosing the right one is crucial. I used `.pem`.

---

## Launching a Public Server

I had to change my EC2 instance's networking settings by editing the VPC — changing it from default to my new VPC — and also selected the right public subnet and firewall: a custom security group.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-ec2_88727bef)

---

## Launching a Private Server

My private server has its own dedicated security group because the public server's security group is internet-facing, meaning it allows HTTP requests. That's not the case for the private server — it only requires SSH access, and only from a specific, secured security group.

My private server's security group source is set to my public security group, meaning we replaced "Anywhere" with our controlled security group. This restricts access to a much smaller group of trusted resources.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-ec2_4a9e8014)

---

## Speeding up VPC Creation

I used an alternative way to set up an Amazon VPC — this time using the VPC wizard to create the complete VPC structure automatically.

A VPC resource map is a visual representation of a VPC and its components.

My new VPC has a CIDR block of `10.0.0.0/16`. It is possible for my new VPC to have the same IPv4 CIDR block as my existing VPC because VPCs are isolated spaces on the cloud — there is no conflict in having the same CIDR block unless I am going to use VPC peering.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-ec2_1cbb1b88)

### Tips for Using the VPC Resource Map

When determining the number of public subnets in my VPC, I only had two options: 0 and 2. This is because I selected 2 Availability Zones — one public subnet per AZ — so the wizard ensures redundancy, leading to high availability of public resources.

NAT gateways let instances in private subnets access the internet for updates and patches, while blocking unsolicited inbound traffic.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-ec2_8ee57662)

---
