# Creating a Private Subnet

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-private)  
**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated space on the cloud for anyone to manage resources under their own custom rules and regulations.

### How I used Amazon VPC in this project

I used Amazon VPC to create and manage subnets with proper security measures and configurations.

### Reflections

- **Surprise:** The project was simpler to complete than expected.
- **Time taken:** ~30 minutes

---

## Private vs Public Subnets

Public subnets are user-facing and have a direct connection to the internet. Private subnets, on the other hand, are isolated and accessible only under specific rules and regulations.

**Why use private subnets?**  
Resources like databases should not be directly accessible from the internet. Placing them in a private subnet keeps them protected while still reachable from within the VPC.

> Note: Private and public subnets cannot share the same CIDR block.

![Subnet Diagram](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-private_afe1fdbd)

---

## Dedicated Route Table

By default, the private subnet inherits the public route table created for the public subnet. That table includes a route to an Internet Gateway, which is inappropriate for private resources.

**Solution:** Create a dedicated private route table that contains only a single local route — allowing traffic only within the VPC.

![Private Route Table](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-private_b4b904b5)

---

## Network ACL (NACL)

By default, the private subnet is associated with the VPC's default NACL, which allows all inbound and outbound traffic — too permissive for a private subnet.

**Solution:** Create a dedicated NACL for the private subnet. This new NACL has two rules:
- **Inbound:** Deny all traffic
- **Outbound:** Deny all traffic

This ensures no unexpected traffic can reach or leave the private subnet until explicit allow rules are added.

![Private NACL](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-private_1ed2cb07)
