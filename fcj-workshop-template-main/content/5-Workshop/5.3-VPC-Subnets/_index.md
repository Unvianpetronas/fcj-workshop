---
title: "3. Create VPC-Subnet"
date: "`r Sys.Date()`"
weight: 503
chapter: false
pre: " <b> 5.3. </b> "
---

In this component, you will create your own Virtual Private Cloud (VPC) and define two subnets:

- 1 Public Subnet (Internet accessible)
- 1 Private Subnet (no direct Internet access)

### 3.1 Create the VPC

1. Open **AWS Management Console**
2. Navigate to **Services → VPC**
3. Click **Create VPC**
4. Select **VPC Only**
5. Enter the details:
    - **Name tag:** `Workshop-VPC`
    - **IPv4 CIDR:** `10.0.0.0/16`
6. Keep all other settings at default
7. Click **Create VPC**

![VPC Creation Menu](/images/5-Workshop/3-VPC-Subnets/vpc-creation.png)

---

### 3.2 Create Public Subnet

1. In the left menu, click **Subnets**
2. Click **Create subnet**
3. Configure as follows:
    - **VPC:** `Workshop-VPC`
    - **Subnet name:** `Public-Subnet`
    - **Availability Zone:** (any, example: `ap-southeast-1a`)
    - **IPv4 CIDR block:** `10.0.1.0/24`
4. Click **Create subnet**

![Subnet Creation Menu](/images/5-Workshop/3-VPC-Subnets/public-subnet-creation.png)

---

### 3.3 Create Private Subnet

1. Click **Create subnet** again
2. Fill in the values:
    - **VPC:** `Workshop-VPC`
    - **Subnet name:** `Private-Subnet`
    - **Availability Zone:** (can be same or different)
    - **IPv4 CIDR block:** `10.0.2.0/24`
3. Click **Create subnet**

> This process is similar to 3.2 with only minor differences. Please see 3.2 section for reference.

---

### 3.4 Enable Auto-Assign Public IP (Only for Public Subnet)

1. Go to **Subnets**
2. Select **Public-Subnet**
3. Click **Actions → Edit subnet settings**
4. Enable:
    - ☑ **Auto-assign public IPv4**
5. Click **Save**

![Subnet Setting Menu](/images/5-Workshop/3-VPC-Subnets/public-subnet-IP-assign.png)

---

**Expected result:**
You now have a VPC with two subnets ready to use.

Components created so far:

| Resource           | Name           | CIDR        |
|--------------------|----------------|-------------|
| VPC                | Workshop-VPC   | 10.0.0.0/16 |
| Subnet 1 (Public)  | Public-Subnet  | 10.0.1.0/24 |
| Subnet 2 (Private) | Private-Subnet | 10.0.2.0/24 |