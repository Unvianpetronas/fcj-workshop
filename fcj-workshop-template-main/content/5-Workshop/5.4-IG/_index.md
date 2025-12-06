---
title: "4. IG"
date: "`r Sys.Date()`"
weight: 504
chapter: false
pre: " <b> 5.4. </b> "
---

The Internet Gateway (IGW) allows resources inside your **Public Subnet** to send and receive traffic from the Internet.
Without it, even a subnet with public IP will still be isolated.

### 4.1 Create an Internet Gateway

1. In the AWS Console, open **VPC** service
2. On the left panel, select **Internet Gateways**
3. Click **Create internet gateway**
4. Fill in:
    - **Name tag:** `Workshop-IGW`
5. Click **Create internet gateway**

![Internet Gateway Creation Menu](/images/5-Workshop/4-IG/IG-creation.png)

---

### 4.2 Attach Internet Gateway to VPC

1. After IGW is created, click **Actions → Attach to VPC**
2. Choose **Workshop-VPC**
3. Click **Attach internet gateway**

![Internet Gateway Attach Menu](/images/5-Workshop/4-IG/IG-attach-vpc.png)

---

### Result Check

You should now see:

| Resource     | Status                   |
|--------------|--------------------------|
| Workshop-IGW | Attached to Workshop-VPC |

![Internet Gateway List](/images/5-Workshop/4-IG/IG-list.png)