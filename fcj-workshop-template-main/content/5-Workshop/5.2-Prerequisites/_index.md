---
title: "2. Prerequisites"
date: "`r Sys.Date()`"
weight: 502
chapter: false
pre: " <b> 5.2. </b> "
---


Before starting the workshop, ensure you have access to the following:

### Accounts & Access

- An **AWS Account**
- **IAM User** with permissions to:
    - Amazon VPC
    - Amazon EC2
    - Elastic IP
    - NAT Gateway
    - Internet Gateway
- Recommended policy: **AdministratorAccess** (for lab purposes only)

> If you are using an IAM user, make sure you have access keys or console login credentials.

### Cost Notice

This workshop includes creating a **NAT Gateway**, which is a billable resource.
To minimize cost, you should delete all created resources during the cleanup step.

_expected workshop cost: ~ few USD if resources are cleaned up the same day_

### Tools Required

| Tool                          | Usage                                                          |
|-------------------------------|----------------------------------------------------------------|
| AWS Management Console        | Main interface for building VPC, Subnets, Route Tables         |
| SSH client (PuTTY / Terminal) | Used later to connect to EC2 instances                         |
| Key Pair                      | Needed for EC2 login (you will create one during the workshop) |

### Region Selection

We recommend using the region closest to you for lower latency.  
Example: **ap-southeast-1 (Singapore)** for Vietnam users.

> You must keep the same region for all steps in this workshop.