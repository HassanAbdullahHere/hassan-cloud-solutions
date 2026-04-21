
# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-monitoring)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

## Project Overview

### What is Amazon VPC?

Amazon VPC is an isolated network space in the cloud that lets companies manage resources under specific security rules and access controls.

### How I used Amazon VPC in this project

I used Amazon VPC to attach Flow Logs to a subnet and analyze the captured traffic using CloudWatch Log Insights.

### One thing I didn't expect

The level of detail in the flow log metrics — each entry shows exactly which IP communicated with which, on what port, and whether the traffic was accepted or rejected.

### Time taken

60 minutes

---

## Project Steps

### Part 1 — Setup

| Step | Description |
|------|-------------|
| 1 | Set up two VPCs with unique CIDR blocks |
| 2 | Launch EC2 instances in each VPC |
| 3 | Configure VPC Flow Logs |
| 4 | Set IAM permissions for Flow Logs to write to CloudWatch |

### Part 2 — Testing & Analysis

| Step | Description |
|------|-------------|
| 5 | Ping testing and connectivity troubleshooting |
| 6 | Set up VPC peering connection |
| 7 | Analyze flow logs in CloudWatch |

---

## Multi-VPC Architecture

I started by launching two VPCs using the visual mapping tool with the following CIDR blocks:

| VPC | CIDR Block |
|-----|------------|
| VPC 1 | 10.1.0.0/16 |
| VPC 2 | 10.2.0.0/16 |

The CIDR blocks must be unique and non-overlapping so that VPC peering can route traffic between them without conflicts.

### EC2 Instances

I launched one EC2 instance in each VPC's subnet. Both instances' security groups allow **SSH** and **ICMP** from anywhere so that ping tests generate analyzable flow log traffic.

![EC2 instances in each VPC](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_e7fa8775)

---

## Flow Logs Setup

**Flow logs** record metadata about traffic flowing through a VPC — including source/destination IPs, ports, protocol, and whether traffic was accepted or rejected.

**Log groups** are CloudWatch containers (like folders) that organize related log streams.

### Flow log for VPC 1

I created a flow log on VPC 1 and directed it to a CloudWatch log group.

![Flow log configuration](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Role

Flow Logs need permission to write to CloudWatch. I created:

- **IAM Policy** — grants `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents` on CloudWatch.
- **IAM Role** — attached the policy and configured a custom trust policy so only the VPC Flow Logs service can assume it.

A **custom trust policy** restricts role assumption to a specific AWS service (`vpc-flow-logs.amazonaws.com`), preventing any other service from using it.

![IAM role and policy](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_4334d777)

---

## Connectivity Troubleshooting

### Initial ping test — failed

My first ping from Instance 1 to Instance 2's **private IP** got no replies, indicating missing routing between the VPCs.

![Failed ping test](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_99d4ba42)

Pinging Instance 2's **public IP** succeeded, confirming Instance 2's security group and internet connectivity were correctly configured — the issue was cross-VPC routing.

### Root cause

VPC 1's route table had no route for the `10.2.0.0/16` CIDR — the VPC peering connection had not been set up yet.

### Fix — VPC Peering

I created a VPC peering connection between VPC 1 and VPC 2, then updated both VPCs' route tables to add routes pointing to the peer connection.

![Updated route tables with peering](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_7316a13d)

### Ping test — success

After the route table updates, Instance 1 received ping replies from Instance 2's private IP, confirming the peering connection was working.

![Successful ping test](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing Flow Logs

Each flow log entry records:

| Field | Description |
|-------|-------------|
| Flow log ID | Unique identifier for the flow |
| Source IP | Origin of the traffic |
| Destination IP | Target of the traffic |
| Port | Port used for the connection |
| Action | `ACCEPT` or `REJECT` |

From the captured logs, I could see accepted packets from Instance 2 (VPC 2) arriving at VPC 1's subnet, and rejected packets from other sources.

![Flow log entries](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_d116818e)

---

## CloudWatch Logs Insights

**Logs Insights** is a CloudWatch feature for querying log data using a SQL-like syntax. It lets you filter, aggregate, and sort log entries to identify traffic patterns or troubleshoot issues.

### Query I ran

```sql
stats sum(bytes) as bytesTransferred by srcAddr, dstAddr
| sort bytesTransferred desc
| limit 10
```

This query surfaces the **top 10 source-destination pairs by bytes transferred**, making it easy to spot which hosts generated the most traffic.

![Logs Insights results](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-networks-monitoring_3e1e79a1)

---
