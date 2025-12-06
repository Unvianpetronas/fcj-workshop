---
title: "Overview"
date: "`r Sys.Date()`"
weight: 501
chapter: false
pre: " <b> 5.1. </b> "
---

# Build a Basic AWS VPC with Public & Private Subnets

In this workshop, you will build a simple network foundation in AWS using **Amazon VPC (Virtual Private Cloud)**.
The goal is to help beginners understand how networking works in the cloud, and how different components connect
together to form a secure, controlled environment.

You will create a VPC with two subnets:

+ A **Public Subnet** – allows resources (like EC2 instances) to access the Internet directly.
+ A **Private Subnet** – isolated from the public Internet by default for better security.

To enable the private subnet to download packages and access online services without being exposed publicly, you will
configure:

+ An **Internet Gateway (IGW)** – allows public subnet traffic to flow to/from the Internet.
+ A **NAT Gateway** – lets instances in the private subnet access the Internet *indirectly*, without assigning a
  public IP.

By the end, you will spin up two EC2 instances — one in each subnet — and test communication between them while
observing the role of routing and gateways.

This hands-on workshop teaches essential AWS networking fundamentals through guided steps and console navigation.
Perfect for beginners who want to build confidence working with VPCs.