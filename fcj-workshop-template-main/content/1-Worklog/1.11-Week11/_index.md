---
title: "Week 11 Worklog"
date: "`r Sys.Date()`"
weight: 111
chapter: false
pre: " <b> 1.11. </b> "
---



### Week 11 Objectives:

* Event participate with DevOps on AWS subject
* Fundamentals to set up IaC (Infrastructure as Code) with Terraform
* Frontend React With setting page for email sending every day
* Document for how we score the architecture base on metadata get on AWS
* Improve email functional and API, set up production on SES

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                           | Start Date | Completion Date | Reference Material |
| :--- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------|:----------------| :--- |
| 2 | - **Professional Development:** <br> - Attend "DevOps on AWS" event <br> - Network with industry peers and take notes on best practices                        | 11/17/2025 | 11/17/2025      | Event Agenda / Notes |
| 3 | - **Infrastructure as Code (IaC):** <br> - Install Terraform and configure AWS Provider <br> - Write basic `.tf` files to define simple resources (EC2/S3)     | 11/18/2025 | 11/18/2025      | Terraform Registry / HashiCorp Docs |
| 4 | - **Frontend Development:** <br> - Build "Email Settings" page in React <br> - Implement UI forms for scheduling daily emails                                  | 11/19/2025 | 11/19/2025      | React Docs / Material UI |
| 5 | - **Backend & Email Service:** <br> - Refactor Email API for better performance <br> - Request production access for AWS SES and configure domain verification | 11/20/2025 | 11/20/2025      | AWS SES Developer Guide |
| 6 | - **Documentation:** <br> - Write logic documentation for Architecture Scoring <br> - Define how AWS metadata maps to scoring criteria                         | 11/21/2025 | 11/21/2025      | Internal Wiki / AWS CloudWatch Docs |


### Week 11 Achievements:

* **DevOps Knowledge:**
  * Participated in the **DevOps on AWS** event, gaining insights into modern CI/CD pipelines and infrastructure automation.

* **Infrastructure as Code (Terraform):**
  * Successfully set up the Terraform environment.
  * Understood core concepts including Providers, Resources, State files, and `plan`/`apply` workflows.

* **Frontend Implementation:**
  * Completed the **Email Settings Page** using React.
  * Users can now configure daily email schedules via the UI.

* **AWS SES Production Setup:**
  * Optimized the email sending API logic.
  * Successfully moved AWS SES from Sandbox to **Production** mode, enabling email delivery to unverified addresses.

* **Scoring Logic Documentation:**
  * Finalized the documentation detailing how the system scores architecture quality based on retrieved AWS metadata.