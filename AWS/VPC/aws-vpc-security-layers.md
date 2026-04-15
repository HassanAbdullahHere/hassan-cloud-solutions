# VPC Traffic Flow and Security

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-security)  
**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Project Banner](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-security_92b0b0b4)

---

## Project Overview

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is a private, isolated space on the cloud for managing and hosting your own resources securely.

### How I used Amazon VPC in this project

In this project, I used Amazon VPC to create a subnet for personal resources, then layered on multiple security controls: a security group, a Network ACL, and a route table connected to an internet gateway for external communication.

### One thing I didn't expect

I didn't expect VPC to have multiple distinct layers of security — each operating at a different scope (instance vs. subnet level).

### Time to complete

50 minutes

---

## Route Tables

A route table acts like a GPS for resources within a VPC — it directs traffic to the correct destination. It determines whether communication stays internal (within the VPC) or routes externally through an internet gateway (IGW).

Route tables are required to make a subnet public. By default, subnets are isolated within the VPC and need an IGW paired with a route table entry to communicate with the internet.

![Route Table](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-security_0a07b191)

---

### Route Destination and Target

Each route is defined by two fields:

| Field | Description |
|-------|-------------|
| **Destination** | The IP range (CIDR block) the traffic is intended for |
| **Target** | The gateway or resource that handles the traffic (e.g., `local`, IGW) |

In my route table, the internet-bound route was configured with:
- **Destination:** `0.0.0.0/0` (all IPs)
- **Target:** My internet gateway (IGW)

---

## Security Groups

Security groups are instance-level firewalls that control inbound and outbound traffic for individual resources.

### Inbound Rules

Inbound rules define what traffic is allowed to reach the resource. I configured an inbound rule that allows all IPv4 addresses over the HTTP protocol.

### Outbound Rules

Outbound rules define where a resource is allowed to send traffic. By default, my security group's outbound rule allows all IPv4 traffic.

![Security Group](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-security_92b0b0b4)

---

## Network ACLs

Think of Network ACLs as traffic officers stationed at every entry and exit point of a subnet — they inspect each data packet against a rule table before allowing it through.

### Security Groups vs. Network ACLs

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| **Scope** | Instance level | Subnet level |
| **Rule evaluation** | All rules evaluated | Rules evaluated in number order |
| **State** | Stateful | Stateless |

---

### Default vs. Custom Network ACLs

**Default ACL:** Inbound and outbound rules allow all traffic from everywhere.

**Custom ACL:** Inbound and outbound rules are automatically set to deny all traffic — rules must be explicitly added. The first default deny rule is assigned rule number 100.

![Network ACL](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-security_4faeb056)

