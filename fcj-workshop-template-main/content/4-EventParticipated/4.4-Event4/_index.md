---
title: "Event 4"
date: "`r Sys.Date()`"
weight: 404
chapter: false
pre: " <b> 4.4. </b> "
---

# EVENT REPORT: DEVOPS ON AWS


**Event Objective:** To provide comprehensive knowledge on DevOps Culture, principles, performance metrics, and the suite of AWS DevOps services for building CI/CD pipelines, IaC, and Observability.

---

## MORNING SESSION (8:30 AM – 12:00 PM): CI/CD & INFRASTRUCTURE AS CODE

### 1. Welcome & DevOps Mindset (8:30 – 9:00 AM)

- The event began with a quick recap from the previous AI/ML session, setting the context for moving models/applications into production.
- The focus was on DevOps culture and principles, emphasizing collaboration between Development (Dev) and Operations (Ops).
- Introduction to key performance metrics such as **DORA** (Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate) and **MTTR** (Mean Time To Recovery).

### 2. AWS DevOps Services – Building the CI/CD Pipeline (9:00 – 10:30 AM)

This section delved into the AWS CodeFamily suite of tools to automate Continuous Integration (CI) and Continuous Deployment (CD):

- **Source Control:** Using AWS CodeCommit and discussing source code management strategies like GitFlow and Trunk-based.
- **Build & Test:** Configuring CodeBuild to automate compilation and running testing pipelines.
- **Deployment:** Analyzing advanced deployment strategies with CodeDeploy, including Blue/Green, Canary, and Rolling updates.
- **Orchestration:** Utilizing CodePipeline to automate the entire process from commit to production.
- **Demo:** A visual demonstration of a complete CI/CD pipeline on AWS.

### 3. Infrastructure as Code (IaC) (10:45 AM – 12:00 PM)

IaC is the foundation of modern DevOps, ensuring environment consistency and reproducibility:

- **AWS CloudFormation:** Learning about Templates, Stacks, and how to use Drift Detection to identify discrepancies between the actual state and the source code.
- **AWS CDK (Cloud Development Kit):** Introducing CDK as a more modern approach, using familiar programming languages (TypeScript, Python,...) to define infrastructure via Constructs and reusable patterns.
- **Demo & Discussion:** Demonstrating infrastructure deployment using both CloudFormation and CDK, along with a discussion on criteria for choosing the right IaC tool.

---

## AFTERNOON SESSION (1:00 PM – 5:00 PM): CONTAINER, OBSERVABILITY & BEST PRACTICES

### 4. Container Services on AWS (1:00 – 2:30 PM)

- **Docker Fundamentals:** Review of basic concepts related to Microservices and Containerization.
- **Amazon ECR (Elastic Container Registry):** The service for storing container images, including image scanning features and lifecycle policies.
- **Amazon ECS & EKS:** Comparing the two main container orchestration services: ECS (simpler, AWS integrated) and EKS (Kubernetes-based). Analysis of deployment and scaling strategies.
- **AWS App Runner:** A simplified solution for container deployment, focusing on code over infrastructure.
- **Demo & Case Study:** Illustrating microservices architecture deployment and comparing the services.

### 5. Monitoring & Observability (2:45 – 4:00 PM)

- **CloudWatch:** The core tool for collecting metrics, logs, alarms, and dashboards across the entire AWS ecosystem.
- **AWS X-Ray:** Provides Distributed Tracing to track request flow across microservices, helping to identify bottlenecks and performance issues.
- **Demo:** Setting up a comprehensive observability system (Full-stack observability).
- **Best Practices:** Best practices for Alerting, dashboards, and the on-call process.

### 6. DevOps Best Practices & Case Studies (4:00 – 4:45 PM)

- **Advanced Deployment Strategies:** Discussion on Feature flags and A/B testing within the deployment pipeline.
- Automated testing and deep integration with CI/CD.
- **Incident Management:** Managing incidents and the importance of Postmortems for learning and improvement.
- **Case Studies:** Lessons learned from startups and large enterprises that have undergone successful DevOps transformations.

### 7. Q&A & Wrap-up (4:45 – 5:00 PM)

- Addressing in-depth questions.
- Providing information on DevOps career pathways and relevant AWS certifications.

---

## DETAILED SESSION BREAKDOWN

### Morning: CI/CD & Infrastructure as Code (8:30 AM – 12:00 PM)

| Time | Session Focus | Key Takeaways & AWS Services Covered |
|------|---------------|-------------------------------------|
| 8:30 – 9:00 AM | DevOps Culture & Mindset | Context setting from previous AI/ML session. Emphasis on collaboration (Dev/Ops). Introduction to DORA (Deployment Frequency, Lead Time, MTTR, CFR) and MTTR performance metrics. |
| 9:00 – 10:30 AM | Building the CI/CD Pipeline with AWS CodeFamily | Automation of Continuous Integration/Deployment. Discussion of Source Control (AWS CodeCommit, GitFlow/Trunk-based). Configuring CodeBuild for compilation/testing. Advanced deployment strategies (Blue/Green, Canary, Rolling updates) with CodeDeploy. Orchestrating the entire workflow via CodePipeline. Session included a practical demo. |
| 10:45 AM – 12:00 PM | Infrastructure as Code (IaC) | Importance of IaC for consistency and reproducibility. Detailed exploration of AWS CloudFormation (Templates, Stacks, Drift Detection). Introduction to the modern approach with AWS CDK (Cloud Development Kit) using programming languages, Constructs, and reusable patterns. Session included a demo and discussion on tool selection criteria. |

### Afternoon: Container, Observability & Best Practices (1:00 PM – 5:00 PM)

| Time | Session Focus | Key Takeaways & AWS Services Covered |
|------|---------------|-------------------------------------|
| 1:00 – 2:30 PM | Container Services on AWS | Review of Microservices and Docker basics. Storage and management using Amazon ECR (image scanning, lifecycle policies). Comparison of orchestration services: Amazon ECS (integrated) vs. EKS (Kubernetes). Brief on AWS App Runner for simplified deployment. Session included a demo and case study. |
| 2:45 – 4:00 PM | Monitoring & Observability | Core principles of full-stack observability. CloudWatch for collecting metrics, logs, alarms, and dashboards. AWS X-Ray for Distributed Tracing to identify cross-service performance issues and bottlenecks. Best practices for alerting and on-call processes. Session included a comprehensive demo. |
| 4:00 – 4:45 PM | DevOps Best Practices & Case Studies | Discussion on advanced deployment techniques (Feature flags, A/B testing). Importance of Automated Testing integration. Strategies for Incident Management and organizational learning through Postmortems. Review of successful DevOps transformations in enterprises. |
| 4:45 – 5:00 PM | Q&A and Wrap-up | Addressed participant questions. Provided guidance on DevOps career paths and recommended AWS certifications. |

---

## GENERAL ASSESSMENT AND APPLICATION

### Assessment

The event covered the entire DevOps lifecycle, from culture, CI/CD process, infrastructure management as code, to container operations and observability. The knowledge delivered was clear, combining theory (DevOps Principles, DORA metrics) and practical tools (CodePipeline, CDK, X-Ray).

### Key Knowledge Acquired

- Mastery of the AWS CodeFamily tools for setting up automated CI/CD pipelines.
- Clear understanding of the pros and cons of CloudFormation and CDK for IaC.
- Ability to differentiate and apply container services (ECS, EKS).
- Setting up proactive monitoring systems using CloudWatch and X-Ray.
- Proficiency in establishing distributed tracing systems for microservices architecture.

### Proposed Next Steps

1. **Practice CI/CD:** Utilize CodeCommit, CodeBuild, and CodePipeline to automate deployment for the current project (if applicable) or a sample application.

2. **Explore CDK:** Start using AWS CDK to define project infrastructure instead of the Console, focusing on creating simple Constructs. Begin migrating infrastructure definition from manual console use to code.

3. **Implement Observability:** Integrate CloudWatch Logs and experiment with AWS X-Ray to trace function calls (API calls) within the project to apply monitoring principles from the start. Establish proactive monitoring and distributed tracing for microservices or API calls in the current development environment.

## Event Photos
![Event4.1.jpg](/images/4-event/Event4.1.jpg)
![Event4.2.jpg](/images/4-event/Event4.2.jpg)
![Event4.3.jpg](/images/4-event/Event4.3.jpg)