_---
title: "9. Cleanup"
date: "`r Sys.Date()`"
weight: 509
chapter: false
pre: " <b> 5.9. </b> "
---_

To avoid unnecessary charges — especially from the **NAT Gateway** — you should remove all created resources after
finishing the workshop.

Follow the steps below in order:

---

### 9.1 Terminate EC2 Instances

1. Open **EC2 Console**
2. Select both `EC2-Public` and `EC2-Private`
3. Click **Instance state → Terminate** (This process may take some time)
4. Confirm termination

![EC2 instances termination](/images/5-Workshop/9-Cleanup/ec2-instance-termination.png)

---

### 9.2 Delete NAT Gateway

1. Go to **VPC Console → NAT Gateways**
2. Select `Workshop-NAT`
3. Click **Actions → Delete NAT Gateway** (This process may take some time)
4. Confirm the action

> Wait until NAT Gateway shows as **Deleted** before moving on.

![EC2 instances termination](/images/5-Workshop/9-Cleanup/NAT-termination.png)

---

### 9.3 Release Elastic IP

After NAT Gateway is deleted:

1. Go to **Elastic IPs**
2. Select the allocated address
3. Click **Actions** → **Release Elastic IP address**
4. Confirm release (empty Elastic IP addresses list)

---

### 9.4 Detach and Delete Internet Gateway

1. In **Internet Gateways**
2. Select `Workshop-IGW`
3. Click **Actions → Detach from VPC**
4. Then **Actions → Delete Internet gateway**
5. Confirm deletion (empty Internet gateways list)

---

### 9.5 Delete Subnets

1. Go to **Subnets**
2. Delete `Public-Subnet`
3. Delete `Private-Subnet`
4. Confirm deletion (empty Subnets list)

---

### 9.6 Delete VPC

1. In **VPC Console**
2. Select `Workshop-VPC`
3. Click **Actions → Delete VPC**
4. Confirm deletion (empty VPCs list)

---

### 9.7 Delete Route Tables

1. Open **Route Tables**
2. Select `Public-RT` → **Delete**
3. Select `Private-RT` → **Delete**
4. Confirm deletion (created route tables no longer exist)

---

### Cleanup Complete