
# Testing VPC Connectivity

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-connectivity)  
**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated space over cloud to manage and secure resources.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to test servers connectivity.

### Reflection

- **Unexpected:** The debugging process was easier than anticipated.
- **Time taken:** ~50 minutes

![Project Overview](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_8ee57662)

---

## Step 1: Connecting to an EC2 Instance

Connectivity is all about how well different parts of your network talk to each other and with external networks.

My first connectivity test was whether I could connect to my Public server within my VPC.

![EC2 Connection](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_88727bef)

### EC2 Instance Connect

EC2 Instance Connect provides direct SSH access to an EC2 instance without needing a key pair file.

**Issue:** My first connection attempt failed because the public security group only had an inbound rule allowing HTTP traffic.

**Fix:** Added a new inbound rule to allow all IPv4 traffic over SSH (port 22).

![EC2 Instance Connect Fix](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_1cbb1b88)

---

## Step 2: Connectivity Between Servers

Ping is a network tool used to check whether your computer can communicate with another device on a network.

**Command used:**
```
ping 10.0.1.238
```

**Observation:** The first ping returned only one line, meaning the packet was sent but no response was received.

![Ping Test](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_defghijk)

### Troubleshooting

The missing responses were caused by incorrect NACL and Security Group rules. Steps taken to resolve:

1. Checked and fixed inbound/outbound rules in the **Network ACL (NACL)**.
2. Added a missing inbound rule in the **Security Group** to allow **ICMP IPv4** traffic from the public subnet.

![Troubleshooting Fix](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_4a9e8014)

---

## Step 3: Connectivity to the Internet

### Ping vs Curl

| Tool | Purpose |
|------|---------|
| **Ping** | Checks connectivity between servers |
| **Curl** | Transfers data to or from a server |

### Testing with Curl

I used `curl` to test connectivity between my public server (inside the VPC) and the external internet.

**Command used:**
```
curl learn.nextwork.org
```

**Result:** The command returned HTML code, confirming the server can successfully reach the internet and render web content.

![Curl Test](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-connectivity_8ee57662)
