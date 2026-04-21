
# VPC Peering

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-peering)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated space on the cloud. It is useful because companies need to manage their resources according to their own rules and regulations.

### What I built

In this project, I set up VPC peering between two VPCs, launched EC2 instances in each VPC, and validated private connectivity between them using ping — including troubleshooting Elastic IP and security group issues along the way.

### One thing I didn't expect

The amount of troubleshooting required — specifically the missing public IP issue and the security group inbound rule for ICMP traffic.

**Time to complete:** ~60 minutes

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_88727bef)

---

## Part 1: Setting Up the Multi-VPC Architecture

### Step 1 — Create Two VPCs

I created two VPCs using the VPC visual mapping tool, each with one public subnet connected to a route table and an Internet Gateway (IGW).

| VPC   | CIDR Block   |
|-------|--------------|
| VPC-1 | 10.1.0.0/16  |
| VPC-2 | 10.2.0.0/16  |

The CIDR blocks must be unique and non-overlapping — overlapping ranges would prevent VPC peering from being established, since private IP addresses must remain unambiguous across both networks.

### Step 2 — Launch EC2 Instances

I launched one EC2 instance in each VPC. No key pairs were needed since access is handled via EC2 Instance Connect directly from the AWS console.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_11111111)

---

## Part 2: Creating the Peering Connection

### Step 3 — Create a Peering Connection

A VPC peering connection is a direct network bridge between two VPCs that allows them to communicate using private IP addresses, without routing traffic over the public internet.

- **Requester** — initiates the peering connection request (VPC-1)
- **Accepter** — reviews and accepts the request to establish the connection (VPC-2)

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_1cbb1b88)

### Step 4 — Update Route Tables

After accepting the peering connection, both VPCs' route tables needed a new route entry:

| Route Table | Destination        | Target              |
|-------------|-------------------|---------------------|
| VPC-1       | 10.2.0.0/16       | VPC-1 <> VPC-2 peering |
| VPC-2       | 10.1.0.0/16       | VPC-1 <> VPC-2 peering |

Without these routes, traffic has no path to the peer VPC even after the peering connection is active.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_4a9e8014)

---

## Part 3: Testing VPC Peering

### Step 5 — Troubleshoot EC2 Instance Connect

When I tried to connect to the EC2 instance via EC2 Instance Connect, I was blocked because the instance had no public IP address assigned.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_7685490c)

**Fix — Assign an Elastic IP Address**

Elastic IP addresses are static public IP addresses that persist across instance stop/start cycles. Associating one with the EC2 instance gave it a stable public IP, which resolved the Instance Connect error.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_45663498)

### Step 6 — Test Connectivity with Ping

From EC2 Instance 1 (VPC-1), I ran:

```bash
ping <VPC-2 Instance Private IP>
```

A successful ping validates the VPC peering connection — it confirms that the instance in VPC-1 can reach the instance in VPC-2 over their private IP addresses.

**Fix — Update Security Group Inbound Rules**

The ping initially failed because VPC-2's EC2 security group had no inbound rule allowing ICMP traffic. I added an inbound rule permitting ICMP from VPC-1's CIDR block (`10.1.0.0/16`), after which the ping succeeded.

![Image](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-peering_7a29d352)

---
