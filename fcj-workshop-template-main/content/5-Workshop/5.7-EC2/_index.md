---
title: "7. EC2"
date: "`r Sys.Date()`"
weight: 507
chapter: false
pre: " <b> 5.7. </b> "
---

You will now deploy two EC2 instances:

| Instance    | Subnet         | Access                                            |
|-------------|----------------|---------------------------------------------------|
| EC2-Public  | Public-Subnet  | SSH directly from Internet                        |
| EC2-Private | Private-Subnet | No public IP — accessible only through Public EC2 |

Both will use the same AMI and instance type for simplicity.

---

### 7.1 Create a Key Pair

1. Go to **EC2 Console**
2. Left panel → **Key Pairs**
3. Click **Create key pair**
4. Set:
    - **Name:** `Workshop-Key`
    - **Type:** RSA
    - **Format:** `.pem` (Linux/Mac) or `.ppk` (Windows PuTTY)
5. Download and store securely

![Key Pair Creation Menu](/images/5-Workshop/7-EC2/ec2-keypair-creation.png)

> _This key will be used later for SSH._

---

### 7.2 Launch Public EC2 Instance

1. In EC2 Console → Click **Instances** → Click **Launch instances**
2. Name: `EC2-Public`
3. Select AMI:
    - **Amazon Linux**
4. Instance type: **t3.micro** (Free tier eligible)
5. Select existing key pair: `Workshop-Key`
6. Network settings (Click **Edit** to show menu):
    - **VPC:** Workshop-VPC
    - **Subnet:** Public-Subnet
    - **Auto-assign Public IP:** Enabled
7. Configure security group:
    - New SG name: `Public-EC2-SG`
    - Inbound rule:
        - Type: **SSH**
        - Source Type: `Custom`
        - Source: `0.0.0.0/0`
8. Leave everything else as default.
9. Launch instance

![EC2 Instance Launch Menu](/images/5-Workshop/7-EC2/ec2-instance-creation.png)

---

### 7.3 Launch Private EC2 Instance

1. In EC2 Console → Click **Instances** → Click **Launch instances** again
2. Name: `EC2-Private`
3. AMI + instance type = same as public EC2
4. Use key pair: `Workshop-Key`
5. Network settings:
    - **VPC:** Workshop-VPC
    - **Subnet:** Private-Subnet
    - **Auto-assign Public IP:** Disabled
6. Security Group:
    - Name: `Private-EC2-SG`    
    - Type: **SSH**
    - Source Type: `Custom`
    - Source: `Public-EC2-SG` **(not the Internet)**

📸 _Screenshot here: Private EC2 config with No Public IP_
![EC2 Private Instance Launch Menu](/images/5-Workshop/7-EC2/ec2-instance-private-creation.png)

---

### 7.4 Verify Instances

| Instance    | IP              | Expected                      |
|-------------|-----------------|-------------------------------|
| EC2-Public  | Has IPv4 Public | Can SSH from your machine     |
| EC2-Private | No Public IP    | Only reachable via Public EC2 |

![EC2 Instances List](/images/5-Workshop/7-EC2/ec2-instance-list.png)