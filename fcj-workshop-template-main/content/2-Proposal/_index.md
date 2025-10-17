---
title: "Proposal"
date: "2025-10-13"
weight: 200
chapter: false
pre: "  2.  "
---
# AWS Cloud Health Dashboard 

---

## 1. Executive Summary

**AWS Cloud Health Dashboard** is a **production-grade, multi-tenant SaaS platform** with **full DevSecOps implementation** that enables businesses to monitor and optimize AWS infrastructure for multiple clients from a single centralized system.

**Key Highlights:**

- **DevSecOps Architecture**: Complete CI/CD pipeline with automated security scanning
- **Multi-tenant architecture**: Monitor 10-50+ client AWS accounts from one platform
- **Platform cost**: $23-33/month (Year 1) with DevSecOps features
- **Per-client cost**: ~$0.19/month in data storage
- **Security-first design**: AWS Secrets Manager + KMS encryption
- **Automated deployment**: CodePipeline + CodeBuild with SSH deployment
- **5 DynamoDB tables**: Optimized data model with client isolation
- **Redis caching**: 80% reduction in database read costs
- **Email notification system**: AWS SES integration for critical alerts

**Technology Stack:** FastAPI (Python) + React + DynamoDB + Redis + AWS Secrets Manager + KMS + CloudWatch + CloudTrail + CodePipeline + CodeBuild + EC2 t3.micro

---

## 2. Problem Statement

**Current Challenges:**

Businesses managing AWS infrastructure for multiple clients face:

- **No centralized monitoring**: Must log into each client's AWS console separately
- **Scattered security alerts**: Critical GuardDuty findings go unnoticed
- **Cost visibility gaps**: Difficult to track and optimize costs across clients
- **Insecure credential management**: AWS access keys stored in databases or config files
- **No automated deployment**: Manual deployment leads to errors and downtime
- **Lack of security scanning**: Vulnerabilities deployed to production
- **No audit logging**: Cannot track security events and API calls
- **Manual monitoring**: 30+ minutes daily per client

**Our Solution:**

Cloud Health Dashboard provides a **single DevSecOps-enabled platform** with:

**DevSecOps Architecture**
- Automated CI/CD pipeline (CodePipeline + CodeBuild)
- Security scanning (SAST with Bandit, dependency scanning with Safety)
- Automated deployment via SSH
- Infrastructure monitoring with CloudWatch
- Audit logging with CloudTrail
- Secrets management with AWS Secrets Manager + KMS

**Multi-Tenant Architecture**
- One platform monitors 10-50+ client AWS accounts
- AWS Secrets Manager for credential storage (KMS encrypted)
- Complete data isolation between clients
- Automatic worker management per client

**Centralized Monitoring**
- Single dashboard for all clients
- Real-time infrastructure health
- Historical data retention (30-365 days)
- 15+ AWS services monitored
- Redis caching for performance

---

## 3. Solution Architecture

### **Architecture Overview**

![new.png](/images/2-Proposal/new.png)

### **Data Flow**

```
1. DEVELOPER WORKFLOW (DevSecOps)
   Developer → Git push → GitHub → CodePipeline triggered
   → CodeBuild runs:
     • Install dependencies
     • Run tests (pytest)
     • Security scan (Bandit, Safety)
     • Build frontend
     • SSH to EC2 → Deploy
   → CloudWatch logs everything

2. CLIENT SIGNUP
   Customer → API → Validate AWS keys → Store in Secrets Manager (KMS encrypted)
   → Store metadata in DynamoDB 

3. EMAIL VERIFICATION
   Customer → Enter email → Click verify → Email send verify link →Click Link → Verify token → Mark email verified → CloudTrail logs event

4. WORKER MANAGER (Multi-Tenant)
   Every 15 min → Fetch active clients from DynamoDB
   → For each client:
     • Get credentials from Secrets Manager
     • Spawn worker if not running
     • Collect AWS data
     • Cache in Redis (5 min TTL)
     • Store in DynamoDB

5. DATA COLLECTION (Per Client)
   Worker → Secrets Manager (get credentials)
   → Client AWS API → Collect metrics
   → Cache in Redis → Store in DynamoDB (partitioned by aws_account_id)
   → If critical finding → Send email alert (SES)
   → CloudWatch logs all operations

6. DASHBOARD DISPLAY
   Customer login → FastAPI checks Redis cache
   → Cache HIT: Return cached data (< 100ms)
   → Cache MISS: Query DynamoDB (filtered by aws_account_id)
   → Store in Redis → Return data

7. SECURITY & AUDIT
   All API calls → CloudTrail logging
   Credentials access → CloudWatch alarm
   Failed auth attempts → Email alert
   KMS key usage → Audit trail
```

---

## 4. Key Features

### **DevSecOps Implementation**

**Automated CI/CD Pipeline:**
- GitHub integration for source control
- CodePipeline for orchestration (1 pipeline FREE)
- CodeBuild for automated builds (100 mins/month FREE)
- Automated security scanning before deployment
- SSH-based deployment to EC2
- Zero-downtime deployment strategy

**Security Scanning:**
- **Bandit**: Static Application Security Testing (SAST)
- **Safety**: Dependency vulnerability scanning
- Pre-deployment security gates
- Build fails on HIGH severity issues

**Secrets Management:**
- AWS Secrets Manager for credential storage
- KMS encryption for all secrets
- Automatic key rotation support
- No credentials in code or environment variables
- IAM role-based access only

**Monitoring & Logging:**
- CloudWatch for application logs and metrics
- CloudTrail for API audit logging
- Custom metrics for business KPIs
- Automated alerts for anomalies
- 90-day log retention

**Infrastructure Security:**
- Security Groups (network firewall)
- IAM roles with least privilege
- AWS Shield Standard (DDoS protection)
- Encrypted data at rest (KMS)
- Encrypted data in transit (TLS/HTTPS)

### **Multi-Tenant Management**

- Self-service client signup with AWS key validation
- Credentials stored in Secrets Manager (KMS encrypted)
- Automatic worker spawning per client
- Complete data isolation
- Per-client dashboard access
- Redis caching for performance

### **Email Notification System**

- Email verification with secure tokens (24h expiry)
- Critical GuardDuty alerts via AWS SES
- Customizable notification preferences
- HTML email templates
- Email editing in Settings page

### **Infrastructure Monitoring**

- Real-time metrics from 15+ AWS services
- Historical data retention (30+ days)
- Redis caching (80% cost reduction)
- EC2, S3, RDS, Lambda monitoring
- Service health dashboards

### **Cost Optimization**

- Daily cost tracking per service
- Monthly cost trends
- AWS Cost Explorer recommendations
- Budget alerts
- Per-client cost analysis

### **Security Monitoring**

- GuardDuty threat detection (optional)
- Severity-based filtering
- Email alerts for critical findings
- CloudTrail audit logging
- Compliance status tracking

---

## 5. Technical Implementation

### **Technology Stack**

**Backend:**
- Python 3.12+ with FastAPI
- boto3 (AWS SDK)
- asyncio for concurrent workers
- Redis for caching
- pytest for testing

**Frontend:**
- React 18 with Vite
- TanStack Query for data fetching
- Recharts for visualization
- Tailwind CSS for styling

**Database:**
- DynamoDB (5 tables, on-demand pricing)
- Redis (in-memory caching, localhost)

**DevSecOps:**
- GitHub (source control)
- CodePipeline (CI/CD orchestration)
- CodeBuild (automated builds)
- Bandit (SAST security scanner)
- Safety (dependency scanner)

**Security:**
- AWS Secrets Manager (credential storage)
- AWS KMS (encryption keys)
- CloudTrail (audit logging)
- CloudWatch (monitoring & alerts)

**Email:**
- AWS SES for transactional emails
- HTML email templates
- Token-based verification



**Data Isolation:**

- All queries filtered by `aws_account_id`
- DynamoDB partition key includes client identifier
- API endpoints require client authentication
- Credentials isolated in Secrets Manager
- No cross-client data leakage


## 6. Budget Estimation

### **Monthly Platform Costs**

| Service                    | Description                    | Month 1 | Month 2 | Month 3 |
|----------------------------|--------------------------------|---------|---------|---------|
| **EC2 t3.micro**           | 750h Free Tier                 | $0      | $0      | $0      |
| **DynamoDB (5 tables)**    | On-demand, multi-tenant        | $3-4    | $5-8    | $8-12   |
| **AWS Secrets Manager**    | 20 secrets @ $0.40/secret      | $4-6    | $6-8    | $8      |
| **AWS KMS**                | 2-3 keys @ $1/key              | $2      | $2-3    | $2-3    |
| **AWS SES**                | Email sending                  | $0      | $0-1    | $1-2    |
| **CloudWatch**             | Logs + Metrics                 | $0-1    | $1-2    | $2-3    |
| **CloudTrail**             | 1 trail (FREE)                 | $0      | $0      | $0      |
| **CodePipeline**           | 1 pipeline (FREE)              | $0      | $0      | $0      |
| **CodeBuild**              | 100 mins/month (FREE)          | $0      | $0      | $0      |
| **Data Transfer**          | 15GB Free Tier                 | $0      | $0-1    | $1      |
| **S3 Backup**              | ~5GB storage                   | $0      | $0-1    | $1      |
| **Redis**                  | On EC2 (localhost - FREE)      | $0      | $0      | $0      |
| **TOTAL**                  |                                | **$9-13** | **$14-24** | **$23-33** |

### **Cost After Free Tier (Year 2+)**

| Service                    | Monthly Cost |
|----------------------------|--------------|
| EC2 t3.micro               | $8-10        |
| DynamoDB                   | $8-12        |
| Secrets Manager            | $8           |
| KMS                        | $2-3         |
| SES                        | $1-2         |
| CloudWatch                 | $2-3         |
| S3                         | $1           |
| Data Transfer              | $1           |
| **TOTAL Year 2+**          | **$31-40/month** |

### **Per-Client Incremental Cost**

**DynamoDB Storage & Operations (per client/month):**

- **Storage:** ~500MB = $0.125
- **Writes:** 43,200/month = $0.054
- **Reads (with Redis):** 6,000/month = $0.0015 (80% reduction)
- **Total per client: ~$0.19/month**

**Secrets Manager (per client):**
- **Per secret:** $0.40/month
- **Total with Secrets Manager: ~$0.59/month per client**

**Example Scaling:**

- **10 clients:** Platform $23 + (10 × $0.59) = **$28.90/month**
- **20 clients:** Platform $23 + (20 × $0.59) = **$34.80/month**
- **50 clients:** Platform $23 + (50 × $0.59) = **$52.50/month**

### **Cost Optimization Strategies**

- **Redis caching**: Reduces DynamoDB reads by 80%
- **DynamoDB TTL**: Automatic data cleanup (no cost)
- **Batch writes**: Fewer API calls
- **On-demand pricing**: Pay only for usage
- **Free Tier maximization**: 12 months free for EC2, S3, CloudWatch
- **CodeBuild optimization**: Keep builds under 100 mins/month (FREE)
- **Email batching**: Maximize SES free tier

---

## 7. Risk Assessment

| Risk                          | Impact | Probability | Mitigation                                                    |
|-------------------------------|--------|-------------|---------------------------------------------------------------|
| Budget overrun                | Medium | Low         | AWS Budget alerts, Redis caching, DynamoDB on-demand          |
| EC2 downtime                  | High   | Low         | CloudWatch alarms, systemd auto-restart, 98% uptime target    |
| Client data security          | High   | Low         | Secrets Manager + KMS, IAM least privilege, audit logging     |
| CI/CD pipeline failure        | Medium | Low         | CodeBuild retry logic, deployment rollback, health checks     |
| Redis cache failure           | Medium | Low         | Systemd monitor, auto-restart, graceful degradation           |
| DynamoDB hot partitions       | Medium | Low         | Proper partition key design, aws_account_id sharding          |
| Secrets Manager costs         | Medium | Medium      | Monitor usage, optimize secret count, consider alternatives   |
| Email delivery issues         | Medium | Low         | AWS SES monitoring, retry logic, fallback notifications       |
| Security vulnerability in deps| High   | Medium      | Safety scanner in CI/CD, automated updates, security alerts   |
| Scope creep                   | Medium | High        | Strict MVP definition, feature freeze week 8, Phase 2 plan    |

---

## 8. Expected Outcomes

### **Technical Deliverables**

**DevSecOps Platform:**

- Complete CI/CD pipeline (GitHub → CodePipeline → CodeBuild → EC2)
- Automated security scanning (Bandit SAST, Safety dependency scan)
- SSH-based automated deployment
- CloudWatch monitoring and CloudTrail audit logging
- AWS Secrets Manager for credential storage
- KMS encryption for all sensitive data
- Redis caching layer on EC2

**Multi-Tenant SaaS:**

- Client signup with email verification
- 5 DynamoDB tables with proper data isolation
- Automated worker management (10-50 clients)
- Real-time monitoring dashboard per client
- Email notification system
- Settings page with email/notification management

**Performance Targets:**

- **API response time:** <100ms (Redis cache HIT), <2s (cache MISS)
- **Email delivery:** <30 seconds
- **Data collection:** Every 15 minutes per client
- **Platform uptime:** 98-99%
- **Worker startup:** <10 seconds per client
- **Build time:** <5 minutes (under free tier limit)
- **Deployment time:** <3 minutes

**Security & Compliance:**

- Zero credentials in code or environment variables
- All secrets encrypted with KMS
- Email verification before critical alerts
- Complete API audit trail (CloudTrail)
- Data isolation between clients
- IAM least privilege roles
- Automated security scanning in CI/CD

### **Learning Outcomes**

**DevSecOps Mastery:**

- CI/CD pipeline design and implementation
- Automated security scanning (SAST, dependency scanning)
- Infrastructure as Code concepts
- Secrets management best practices
- Audit logging and compliance
- Monitoring and alerting

**AWS Services Mastery:**

- Advanced DynamoDB (multi-tenant, GSI, TTL)
- AWS Secrets Manager + KMS integration
- CloudWatch + CloudTrail
- CodePipeline + CodeBuild
- AWS SES integration
- IAM roles and policies

**SaaS Architecture:**

- Multi-tenant data modeling
- Background worker orchestration
- Secure credential management
- Caching strategies (Redis)
- Email notification system

**Full-Stack Development:**

- FastAPI async programming
- React state management
- Redis integration
- Security-first development
- RESTful API design

### **Portfolio Value**

This project demonstrates:

1. **Production-Grade DevSecOps**
    - Complete CI/CD pipeline
    - Automated security scanning
    - Secrets management with AWS Secrets Manager + KMS
    - Comprehensive monitoring and logging

2. **Enterprise Architecture**
    - Multi-tenant SaaS design
    - Scalable infrastructure (10-100+ clients)
    - Redis caching layer
    - Background worker system

3. **Security Expertise**
    - Zero secrets in code
    - KMS encryption
    - CloudTrail audit logging
    - Automated vulnerability scanning

4. **AWS Proficiency**
    - 14+ AWS services integrated
    - Cost-optimized design
    - Free tier maximization
    - IAM best practices

5. **Cost Efficiency**
    - $23-33/month for platform (Year 1)
    - $0.59/client/month incremental
    - 80% read cost reduction (Redis)

**Key Differentiators:**

- Full DevSecOps implementation (not just a simple app)
- Production-grade security (Secrets Manager + KMS)
- Automated CI/CD with security gates
- Multi-tenant architecture at scale
- Comprehensive monitoring and audit logging
- Cost-efficient Redis caching

---

## 9. Conclusion

**AWS Cloud Health Dashboard** is a **production-grade, DevSecOps-enabled multi-tenant SaaS platform** that demonstrates:

1. **DevSecOps Excellence**
    - Automated CI/CD pipeline (CodePipeline + CodeBuild)
    - Security scanning before every deployment
    - Secrets management with AWS Secrets Manager + KMS
    - Comprehensive monitoring (CloudWatch + CloudTrail)
    - Infrastructure as Code principles

2. **Enterprise Architecture**
    - Multi-tenant design supporting 10-100+ clients
    - Scalable worker system
    - Redis caching for performance
    - Complete data isolation

3. **Security-First Design**
    - Zero secrets in code or config files
    - KMS encryption for all sensitive data
    - CloudTrail audit logging
    - Automated vulnerability scanning

4. **AWS Expertise**
    - Deep integration with 14+ AWS services
    - Cost-optimized infrastructure
    - Security best practices
    - Email notification system (SES)

5. **Business Acumen**
    - Cost-efficient operation ($23-33/month Year 1)
    - Scalable pricing ($0.59/client/month)
    - Professional SaaS features
    - Clear ROI for clients

**Timeline:** 3 months | **Team:** 4 people | **Budget:** $23-33/month (Year 1), $31-40/month (Year 2+)

**This project showcases production-ready DevSecOps implementation, making it an exceptional portfolio piece for cloud engineering and SRE roles.**

---

## Appendices

**A. Services Used (14 AWS Services):**

1. EC2 (Compute)
2. DynamoDB (Database)
3. AWS Secrets Manager (Credential storage)
4. AWS KMS (Encryption)
5. CloudWatch (Monitoring)
6. CloudTrail (Audit logging)
7. CodePipeline (CI/CD)
8. CodeBuild (Automated builds)
9. SES (Email)
10. S3 (Backup)
11. VPC (Networking)
12. Internet Gateway (Connectivity)
13. Security Groups (Firewall)
14. IAM (Access management)
15. Shield Standard (DDoS protection - automatic)

**B. GitHub Repository:** [https://github.com/Unvianpetronas/Cloud_health_dashboard](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**C. Contact Information:**
- **Project Lead:** Truong Quoc Tuan
- **Email:** `unviantruong26@gmail.com`
- **WhatsApp:** `+84 798806545`

---


