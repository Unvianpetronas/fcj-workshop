---
title: "6. RouteTB"
date: "`r Sys.Date()`"
weight: 506
chapter: false
pre: " <b> 5.6. </b> "
---

Route Tables decide how traffic flows inside your VPC.  
In this component, you will:

- Create separate route tables for public and private subnets
- Route Internet traffic from the public subnet → Internet Gateway
- Route private subnet traffic → NAT Gateway for outbound access

---

### 6.1 Create Route Table for Public Subnet

1. In VPC console, open **Route Tables**
2. Click **Create route table**
3. Enter:
    - **Name:** `Public-RT`
    - **VPC:** `Workshop-VPC`
4. Click **Create route table**

![Route Table Creation Menu](/images/5-Workshop/6-RouteTB/RT-creation.png)

---

### 6.2 Add Route to Internet Gateway

1. Select route table **Public-RT**
2. Go to the **Routes** tab
3. Click **Edit routes → Add route**
4. Enter:
    - **Destination:** `0.0.0.0/0`
    - **Target:** `Internet Gateway (Workshop-IGW)`
5. Click **Save changes**

![Route Table add new route](/images/5-Workshop/6-RouteTB/RT-add-route.png)

---

### 6.3 Associate Public Subnet with Public Route Table

1. Still inside Public-RT page
2. Click **Subnet associations**
3. Under **Explicit subnet associations** tab → **Edit subnet associations**
4. Select **Public-Subnet**
5. **Save**

![Route Table add Public Subnet](/images/5-Workshop/6-RouteTB/RT-add-subnet.png)

---

### 6.4 Create Route Table for Private Subnet

1. Click **Create route table**
2. Enter:
    - **Name:** `Private-RT`
    - **VPC:** `Workshop-VPC`
3. Click **Create route table**

> This process is similar to 6.1 with only minor differences. Please see 6.1 section for reference.

---

### 6.5 Add Route to NAT Gateway (Private Subnet Outbound Access)

1. Select **Private-RT**
2. Go to **Routes → Edit routes**
3. Add:
    - **Destination:** `0.0.0.0/0`
    - **Target:** `NAT Gateway (Workshop-NAT)`
4. Save

![Route Table add new route to NAT Gateway](/images/5-Workshop/6-RouteTB/RT-add-route-NAT.png)

---

### 6.6 Associate Private Subnet

1. Go to **Subnet associations** (of **Private-RT** route table)
2. Under **Explicit subnet associations** tab → **Edit subnet associations**
3. Select **Private-Subnet**
4. Save

![Route Table add Private Subnet](/images/5-Workshop/6-RouteTB/RT-add-subnet-private.png)

---

### Result Check

You should now see:

| Route Table | Subnet         | Internet Path    |
|-------------|----------------|------------------|
| Public-RT   | Public-Subnet  | Internet Gateway |
| Private-RT  | Private-Subnet | NAT Gateway      |

At this point, networking backbone is **fully functional**.  
Next up — we deploy EC2 instances to test the security measures.