---
title: "5. NAT"
date: "`r Sys.Date()`"
weight: 505
chapter: false
pre: " <b> 5.5. </b> "
---

The NAT Gateway allows instances in the **Private Subnet** to access the Internet
_without being publicly exposed_. This is how the private EC2 will `yum install`, download updates, etc.

### 5.1 Allocate Elastic IP

1. In the left VPC menu, select **Elastic IPs**
2. Click **Allocate Elastic IP address**
3. Choose default settings
4. Click **Allocate**

![Allocate Elastic IP window](/images/5-Workshop/5-NAT/elastic-IP-allocate.png)

---

### 5.2 Create NAT Gateway

1. In the left VPC panel, go to **NAT gateways**
2. Click **Create NAT gateway**
3. Configure:

| Field                 | Value                           |
|-----------------------|---------------------------------|
| Name                  | `Workshop-NAT`                  |
| Availability mode     | Zonal                           |
| Subnet                | `Public-Subnet`                 |
| Connectivity type     | Public                          |
| Elastic IP allocation | Select the one you just created |

4. Click **Create NAT gateway**

![Create NAT Gateway screen with selected subnet + EIP](/images/5-Workshop/5-NAT/NAT-creation.png)

---

### 5.3 Wait for NAT Gateway to become "Available"

- Status starts as **Pending**
- Wait until it changes to **Available**

![NAT Gateway state](/images/5-Workshop/5-NAT/NAT-state.png)

---

### Result Check

| Resource                     | Status     |
|------------------------------|------------|
| NAT Gateway (`Workshop-NAT`) | Available  |
| Elastic IP                   | Associated |

You’re now ready to route private subnet traffic through NAT.