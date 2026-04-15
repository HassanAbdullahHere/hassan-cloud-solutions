

# Build a Virtual Private Cloud

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-vpc)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Build a Virtual Private Cloud (VPC)

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-vpc_2facf927)

---

## Project Overview

In this project, I created a personal AWS VPC, configured a public subnet, and connected an Internet Gateway. This hands-on work helped me better understand the core building blocks of cloud networking infrastructure.

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is an isolated private network within AWS where you can launch and manage your cloud resources. It gives you full control over your networking environment, including IP address ranges, subnets, route tables, and network gateways.

In this project, I used Amazon VPC to create a subnet and made it publicly accessible using an Internet Gateway.

### Personal Reflection

This project took me around 45 minutes to complete.

---

## Virtual Private Clouds (VPCs)

### What I Did

I created a custom AWS VPC to serve as the foundation for the rest of the project.

### How VPCs Work

A VPC is your private, isolated section of the AWS cloud. It acts as a boundary around your resources, ensuring they are not publicly accessible unless you explicitly configure them to be. Without a VPC, there would be no way to control who can access or manage your resources.

### Why There Is a Default VPC in AWS Accounts

Every AWS account comes with a default VPC already in place. This is because many services — such as EC2 and RDS — require a VPC to operate. Services like S3 and AWS Lambda, however, are managed by AWS and do not require a VPC to function.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-vpc_2facf927)

### Defining IPv4 CIDR Blocks

To set up my VPC, I defined an IPv4 CIDR block. CIDR (Classless Inter-Domain Routing) specifies the range of IP addresses available within my VPC, effectively defining how many resources I can host on my private network.

---

## Subnets

### What I Did

I created a public subnet inside my custom VPC.

### Creating and Configuring Subnets

Subnets are subdivisions of a VPC — think of them as neighbourhoods within a city, where the VPC is the city itself. By default, AWS creates one subnet per Availability Zone in your region.

### Public vs Private Subnets

- **Public subnets** are used for resources that need direct access to the internet (e.g., web servers).
- **Private subnets** are used for sensitive resources that should not be exposed to the public internet (e.g., databases).

For a subnet to be considered public, it must be connected to an Internet Gateway.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-vpc_157c4219)

### Auto-Assigning Public IPv4 Addresses

After creating the subnet, I enabled the **auto-assign public IPv4 address** setting. This ensures that any EC2 instance launched in this subnet automatically receives a public IP address, removing the need to assign one manually.

---

## Internet Gateways

### What I Did

I created an Internet Gateway and attached it to my VPC to enable internet access.

### Setting Up Internet Gateways

An Internet Gateway acts as the bridge between a VPC and the public internet. Without it, resources inside the VPC are completely isolated from the outside world.

Attaching the Internet Gateway to my VPC means that resources in my public subnet can now send and receive traffic from the internet.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-vpc_4ae90410)

---
