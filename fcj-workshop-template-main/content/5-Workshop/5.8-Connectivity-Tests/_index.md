---
title: "8. Connectivity"
date: "`r Sys.Date()`"
weight: 508
chapter: false
pre: " <b> 5.8. </b> "
---

Now that both EC2 instances are running, it's time to validate network behavior and confirm the VPC is functioning
correctly.  
This component verifies three things:

1. Public EC2 is reachable from your local machine
2. Private EC2 is only reachable from the Public EC2 (not directly from the internet)
3. Private EC2 can access the internet through the NAT Gateway

---

### 8.1 Connect to the Public EC2 Instance

1. Open the **EC2 Console** → **Instances**
2. Copy the **Public IPv4** of **EC2-Public**
3. SSH into the instance

> This step is dependent on what type of machine you're using and can vary among different tools. We are using
> Powershell on a Windows 11 OS PC for this Workshop. You can use other alternatives if you prefer.

```bash
ssh -i "Workshop-Key.pem" ec2-user@<PUBLIC_IP_HERE>
```

> Example: ssh -i "Workshop-Key.pem" ec2-user@54.218.10.102

4. Run the command and you will see something like this:

![SSH into public EC2 instance](/images/5-Workshop/8-Connectivity-Tests/ssh-into-public-from-powershell.png)

> Terminal connected to EC2-Public. For security reasons, we had to blurred some details.

### 8.2 SSH from Public EC2 to Private EC2

1. Before you can do this step, you must copy your EC2 instance key/pair into the public EC2 instance **EC2-Public**

> This is not a recommended action by AWS. As it is a great security risk to put access keys onto EC2 instances.
> However, for the sake of simplicity in our purpose of testing connections. You can do this.

2. From the terminal on your machine (logged out of any EC2 intances). Run this:

```bash
scp -i "Workshop-Key.pem" Workshop-Key.pem ec2-user<PUBLIC_IP_HERE>:/home/ec2-user/
```

> This copies your access key into the public EC2 instance **EC2-Public** to be used later in accessing the private EC2
> instance **EC2-Private**

3. Then SSH into **EC2-Public** like you did before in section 8.1 and run:

```bash
chmod 400 Workshop-Key.pem
```

> This limit the permissions of the copied access key, securing it. Allowing you to use it later to SSH into the private
> EC2 instance **EC2-Private**

4. Then run this:

```bash
ssh -i "Workshop-Key.pem" ec2-user@<PRIVATE_IP_HERE>
```

![SSH into private instance from public instance](/images/5-Workshop/8-Connectivity-Tests/ssh-into-private-from-public.png)

> Success, you have successfully SSH into the private EC2 instance **EC2-Private** from the only allowed source of
> connection: The public EC2 instance **EC2-Public**

### 8.3 Test Internet Connectivity from Private Instance

1. Run this:

```bash
ping -c 3 google.com
sudo yum update -y
```

2. Expected behavior:

| Action          | Result                         |
|-----------------|--------------------------------|
| ping google.com | Replies received               |
| yum update      | Packages download successfully |

→ If this works, NAT Gateway routing is confirmed.

![Test internet connection of private instance](/images/5-Workshop/8-Connectivity-Tests/ssh-private-test-internet.png)

> Successful internet connection from the private EC2 instance **EC2-Private**

### 8.4 Validate Security Behavior (Important Check)

| Test                           | Expected Result |
|--------------------------------|-----------------|
| Your PC → Private instance SSH | Blocked         |
| Public EC2 → Private EC2 SSH   | Allowed         |
| Private EC2 → Internet         | Works via NAT   |

→ This confirms public exposure = controlled, and private access = secure + functional.

![SSH into private instance from local machine](/images/5-Workshop/8-Connectivity-Tests/ssh-into-private-from-pc.png)

> As expected. Connection from local machine into the private EC2 instance **EC2-Private** is not allowed. Security
> measures working as intended.

### This concludes the Connectivity Tests