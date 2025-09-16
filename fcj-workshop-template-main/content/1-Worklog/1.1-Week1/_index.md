---
title: "Week 1 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---



### Week 1 Objectives:

* Get acquainted with everyone at FCJ, register for office visits to meet and share knowledge with colleagues.
* Understand basic cloud computing services and how to use Linux.
* Learn about VPC and services available within VPC.
* Explore VPC deployment methods in enterprise and real-world scenarios.

### Tasks to be implemented this week:
| No. | Tasks                                                                                                                                                                                                                                                                     | Start Date | Completion Date | Resources                                                             |
|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|-----------------|-----------------------------------------------------------------------|
| 1   | - Get acquainted with team members, read and note company regulations and policies, as well as review guidelines such as office registration, blog writing guide, worklog,...                                                                                              | 7/09/2025    | 7/09/2025       |
| 2   | - Learn about AWS services offerings <br> - Route 53  <br> - CloudFormation <br> - S3 <br> - EC2 <br> - Database <br>...                                                                                                                                       | 7/09/2025    | 7/08/2025       | https://aws.amazon.com/                                                 |
| 3   | - Create AWS Free Tier account <br> - Watch YouTube about First Cloud Journey <br> <br> - Start the cloud journey with AWS <br> - **Hands-on:** <br>&emsp; + Create AWS account <br>&emsp; + Set up usage budget for AWS <br> &emsp; + Support plans and manage support requests | 8/09/2025    | 8/09/2025       | <https://000001.awsstudygroup.com/> |
| 4   | - Learn VPC basics: <br>&emsp; + subnet <br>&emsp; + route table <br>&emsp; + security group & Net ACLs <br>&emsp; + ... <br> - Peering & Transit gateway <br> - Learn about Elastic IP   <br>                                                                                 | 9/08/2025    | 9/08/2025       | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i/>                                  |
| 5   | - Learn EC2 basics: <br>&emsp; + Instance  <br>&emsp; + AMI <br>&emsp; + keypair  <br>&emsp; + ... <br> - How to access EC2 virtual machine in public subnet <br> - Learn about saving plans   <br>                                                                           | 10/08/2025   | 10/08/2025      | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i>                                  |
| 6   | - **Hands-on:** <br>&emsp; + Create EC2 instance <br>&emsp; + SSH connection <br>&emsp;  + Install Jenkins on EC2 and configure Security group for access <br>&emsp;                                                                                                              | 11/08/2025   | 12/08/2025      | <https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i>                                  |
| 7   | - **Hands-on:** <br>&emsp; + Create VPC <br>&emsp; + Create NAT gateway  <br>&emsp;  + Try connecting VPCs in different regions using peering and Transit gateway                                                                                                                 | 13/08/2025   | 14/08/2025      | <https://000003.awsstudygroup.com/>                                  |


### Week 1 Results Achieved:

## 1. Orientation and Introduction
- Got acquainted with the team and company internship policies
- Understood office registration procedures, blog writing, and worklog processes
- Received guidance on AWS learning roadmap

## 2. AWS Overview
- **Understanding AWS:** Cloud computing platform with Compute, Storage, Networking, Database services
- **Key services mastered:** EC2, S3, VPC, RDS, CloudFormation
- Understood the "pay-as-you-use" model

## 3. AWS Account Setup
- Successfully created AWS Free Tier account
- Configured MFA and IAM users
- Set up Usage Budget and cost alerts

## 4. VPC and Networking
- **Understanding VPC:** Virtual Private Cloud and its role in AWS
- **Key components:** Subnet, Route Table, Security Group, Network ACLs
- **Connectivity:** VPC Peering, Transit Gateway, Elastic IP
- **Custom VPC creation:** CIDR, public/private subnet, route table
- **NAT Gateway:** For private subnet internet access
- **VPC Peering:** Connecting 2 VPCs in same region
- **Transit Gateway:** Managing connections for multiple VPCs

## 5. EC2 Fundamentals
- **EC2 basics:** Instance types, AMI, Key Pair
- **Storage:** EBS volumes
- **Cost optimization:** Savings Plans

## 6. AWS CLI
- Installed and configured AWS CLI with Access Key/Secret Key

## 7. Summary
- **Theory:** Mastered basic AWS concepts
- **Hands-on:** Able to create and manage AWS resources
- **Tools:** Proficient in using AWS Console and CLI
- **Network:** Understanding AWS network architecture and security