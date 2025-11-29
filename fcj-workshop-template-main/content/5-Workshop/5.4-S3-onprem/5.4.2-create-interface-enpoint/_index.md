---
title : "Create an S3 Interface endpoint"
date : "`r Sys.Date()`"
weight : 542
chapter : false
pre : " <b> 5.4.2 </b> "
---

In this section you will create and test an S3 interface endpoint using the simulated on-premises environment deployed as part of this workshop.

1. Return to the Amazon VPC menu. In the navigation pane, choose Endpoints, then click Create Endpoint.

2. In Create endpoint console:
+ Name the interface endpoint
+ In Service category, choose **aws services** 

![6.png](/images/5-Workshop/5.4-S3-onprem/6.png)

3.  In the Search box, type S3 and press Enter. Select the endpoint named com.amazonaws.us-east-1.s3. Ensure that the Type column indicates Interface.

![7.png](/images/5-Workshop/5.4-S3-onprem/7.png)

4. For VPC, select VPC Cloud from the drop-down.
{{% notice warning %}}
Make sure to choose "VPC Cloud" and not "VPC On-prem"
{{% /notice %}}
+ Expand **Additional settings** and ensure that Enable DNS name is *not* selected (we will use this in the next part of the workshop)

![8.png](/images/5-Workshop/5.4-S3-onprem/8.png)

5. Select 2 subnets in the following AZs: us-east-1a and us-east-1b

![9.png](/images/5-Workshop/5.4-S3-onprem/9.png)

6. For Security group, choose SGforS3Endpoint:

![10.png](/images/5-Workshop/5.4-S3-onprem/10.png)

7. Keep the default policy - full access and click Create endpoint

)![11.png](/images/5-Workshop/5.4-S3-onprem/11.png)

Congratulation on successfully creating S3 interface endpoint. In the next step, we will test the interface endpoint.