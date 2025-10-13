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

**AWS Cloud Health Dashboard** is a **multi-tenant SaaS platform** that enables businesses to monitor and optimize AWS infrastructure for multiple clients from a single centralized system. The platform demonstrates enterprise-level architecture while maintaining a cost-efficient operation model.

**Key Highlights:**

- **Multi-tenant architecture**: Monitor 10-50+ client AWS accounts from one platform.
- **Platform cost**: $10-20/month (supports unlimited clients with minimal incremental cost).
- **Per-client cost**: ~$0.50-1.00/month in data storage.
- **Professional features**: Email verification, encrypted credentials, automated alerts, real-time monitoring.
- **5 DynamoDB tables**: Optimized data model with client isolation.
- **Background worker system**: Automated data collection for all clients.
- **Email notification system**: AWS SES integration for critical alerts and verification.

**Technology Stack:** FastAPI (Python) + React + DynamoDB + Redis + AWS SES + EC2 t3.micro

---

## 2. Problem Statement

**Current Challenges:**

Businesses managing AWS infrastructure for multiple clients face:

- **No centralized monitoring**: Must log into each client's AWS console separately.
- **Scattered security alerts**: Critical GuardDuty findings go unnoticed.
- **Cost visibility gaps**: Difficult to track and optimize costs across clients.
- **Manual credential management**: Insecure storage of AWS access keys.
- **No proactive notifications**: Missing critical events in real-time.
- **Time-consuming monitoring**: 30+ minutes daily per client.

**Our Solution:**

Cloud Health Dashboard provides a **single platform to monitor all clients** with:

**Multi-Tenant Architecture**

- One platform monitors 10-50+ client AWS accounts.
- Encrypted AWS credential storage (Fernet encryption).
- Complete data isolation between clients.
- Automatic worker management per client.

**Centralized Monitoring**

- Single dashboard for all clients.
- Real-time infrastructure health.
- Historical data retention (30-365 days).
- 15+ AWS services monitored.

**Email Notification System**

- Email verification with token expiry.
- Critical GuardDuty alerts via AWS SES.
- Beautiful HTML email templates.
- Customizable notification preferences.
- Client can update email in Settings.

**Cost Analysis & Optimization**

- Per-client cost tracking.
- Historical trends and forecasting.
- AWS native recommendations.
- Budget alerts.

**Security & Compliance**

- GuardDuty threat detection.
- Security Hub integration.
- Severity-based filtering.
- Real-time email alerts for critical findings.

**Rule-based Recommendations**

- Cost optimization suggestions.
- Performance improvements.
- Security enhancements.
- Impact-based prioritization.

**ROI & Benefits:**

For Platform Operator:

- Monitor 20 clients for only $15-20/month total.
- Automated data collection (save 10+ hours/week).
- Professional portfolio project.
- Demonstrates enterprise architecture skills.

For Each Client:

- Potential 15-25% cost savings through optimization.
- Immediate critical security alerts via email.
- Historical data visibility.
- Single dashboard instead of multiple AWS consoles.

---

## 3. Solution Architecture

### **Multi-Tenant Architecture Overview**

![diagram-export-10-13-2025-11_02_08-AM.png](/images/2-Proposal/diagram-export-10-13-2025-11_02_08-AM.png)

### **Network Diagram**

![cloud_dashboard.drawio.png](/images/2-Proposal/cloud_dashboard.drawio.png)

### **Data Flow**

```
1. CLIENT SIGNUP
   Customer → API → Encrypt AWS Keys → DynamoDB Clients Table
   → Send Verification Email (AWS SES)

2. EMAIL VERIFICATION
   Customer → Click Link → Verify Token → Mark Email Verified

3. WORKER MANAGER
   Every 5 min → Check for new clients → Start worker per client

4. DATA COLLECTION (per client)
   Worker → Client AWS API → Collect metrics → Store in DynamoDB (with client_id)
   → If critical finding → Send email alert (AWS SES)

5. DASHBOARD DISPLAY
   Customer login → API (filtered by client_id) → Return only their data
```

### **Core Components**

**EC2 t3.micro Instance (Single Server)**:

- **Nginx**: Reverse proxy, SSL termination
- **FastAPI**: RESTful API, authentication, AWS integration
- **React**: Single-page dashboard application
- **Redis**: Caching layer (5-min TTL)
- **Worker Manager**: Multi-client background job orchestration
- **CloudWatch Agent**: Platform monitoring

**5 DynamoDB Tables (Multi-Tenant):**

1. **CloudHealthClients**
   - Stores client accounts with encrypted AWS credentials.
   - Email addresses and verification status.
   - Notification preferences.
   - Client metadata.

2. **CloudHealthMetrics**
   - Time-series metrics from CloudWatch.
   - Partitioned by client_id.
   - 30-day retention (TTL).

3. **CloudHealthCosts**
   - Cost data from Cost Explorer.
   - Per-client cost tracking.
   - 365-day retention.

4. **SecurityFindings**
   - GuardDuty and Security Hub findings.
   - Per-client security alerts.
   - 90-day retention.

5. **Recommendations**
   - Cost, performance, security suggestions.
   - Impact-based prioritization.
   - 180-day retention.

**AWS Services Used:**

- **EC2**: Compute (t3.micro, Free Tier)
- **DynamoDB**: Multi-tenant data storage
- **AWS SES**: Email verification and critical alerts
- **CloudWatch**: Metrics collection
- **Cost Explorer**: Cost analysis
- **GuardDuty**: Threat detection (optional)
- **Security Hub**: Security findings aggregation (optional)
- **S3**: Backup storage

---

## 4. Key Features

### **Multi-Tenant Management**

- Self-service client signup with AWS key validation.
- Encrypted credential storage (Fernet encryption).
- Automatic worker spawning per client.
- Complete data isolation.
- Per-client dashboard access.
- Worker health monitoring.

### **Email Notification System**

- Email verification with secure tokens (24h expiry).
- Critical GuardDuty alerts via AWS SES.
- Customizable notification preferences:
   - Critical security alerts
   - Warning alerts
   - Cost optimization tips
   - Daily infrastructure summary
- Email editing in Settings page.
- Resend verification email option.

### **Infrastructure Monitoring**

- Real-time metrics from 15+ AWS services.
- Historical data retention (30+ days).
- EC2, S3, RDS, Lambda monitoring.
- Custom CloudWatch metrics.
- Service health dashboards.

### **Cost Optimization**

- Daily cost tracking per service.
- Monthly cost trends.
- AWS Cost Explorer recommendations.
- Budget alerts.
- Resource utilization analysis.

### **Security Monitoring**

- GuardDuty threat detection.
- Security Hub findings aggregation.
- Severity-based filtering.
- Email alerts for critical findings.
- Compliance status tracking.

### **Recommendations Engine**

- Cost optimization suggestions.
- Performance improvements.
- Security enhancements.
- Impact-based prioritization.
- Implementation tracking.

---

## 5. Technical Implementation

### **Technology Stack**

**Backend:**

- Python 3.9+ with FastAPI
- boto3 (AWS SDK)
- cryptography (Fernet encryption)
- asyncio for concurrent workers
- Redis for caching

**Frontend:**

- React 18 with Vite
- TanStack Query for data fetching
- Recharts for visualization
- Tailwind CSS for styling

**Database:**

- DynamoDB (5 tables, on-demand pricing)
- Redis (in-memory caching)

**Email:**

- AWS SES for transactional emails
- HTML email templates
- Token-based verification

### **Security Implementation**

**Credential Encryption:**

```python
# Fernet symmetric encryption
from cryptography.fernet import Fernet

# Client AWS keys encrypted before storage
encrypted_key = cipher.encrypt(client_aws_key.encode())
# Stored in DynamoDB, decrypted only when needed
```

**Email Verification:**

```python
# Token generation with expiry
import secrets
from datetime import datetime, timedelta

token = secrets.token_urlsafe(32)
expires = datetime.now() + timedelta(hours=24)
# Stored with client record, verified on click
```

**Worker Manager:**

```python
# Automatic client discovery and worker spawning
import asyncio

for client in active_clients:
    if client_id not in workers:
        worker = CloudHealthWorker(client_provider, client_id)
        workers[client_id] = asyncio.create_task(worker.start())
```

**Data Isolation:**

- All queries filtered by `client_id`.
- DynamoDB partition key includes client identifier.
- API endpoints require client authentication.
- No cross-client data leakage.

---

## 6. Development Roadmap (3 Months)

### **Month 1: Foundation & Core Features**

**Week 1-2: Infrastructure & Multi-Tenant Setup**

- EC2 instance setup with all services.
- DynamoDB 5-table schema creation.
- Client signup API with encryption.
- Worker manager skeleton.
- Basic authentication.

**Week 3-4: Data Collection System**

- CloudWatch metrics collection.
- Per-client worker implementation.
- DynamoDB storage for all 5 tables.
- Redis caching layer.
- Basic dashboard with metrics.

**Deliverable:** Working multi-tenant metrics collection.

---

### **Month 2: Features & Email System**

**Week 5-6: Cost & Security Integration**

- Cost Explorer API integration.
- GuardDuty findings collection.
- Security dashboard.
- Cost analysis charts.

**Week 7-8: Email Notification System**

- AWS SES setup and verification.
- Email verification flow implementation.
- HTML email templates (verification + alerts).
- Critical alert email sending.
- Settings page with email management.
- Notification preferences.

**Deliverable:** Full monitoring + email notifications.

---

### **Month 3: Polish & Production**

**Week 9-10: Recommendations & Testing**

- Recommendations engine implementation.
- Impact-based prioritization.
- End-to-end testing.
- Bug fixes and performance optimization.

**Week 11-12: Production Deployment**

- SSL/TLS setup with Nginx.
- Production monitoring and logging.
- Final documentation.
- Demo preparation.

**Deliverable:** Production-ready multi-tenant SaaS platform.

---

## 7. Budget Estimation

### **Monthly Platform Costs**

| Service             | Description               | Month 1 | Month 2 | Month 3 |
| ------------------- | ------------------------- | ------- | ------- | ------- |
| EC2 t3.micro        | 750h Free Tier            | $0      | $0      | $0      |
| DynamoDB (5 tables) | On-demand, multi-tenant   | $3-4    | $5-8    | $8-12   |
| AWS SES             | Email sending             | $0      | $0-1    | $1-2    |
| CloudWatch          | Metrics + Logs            | $0-1    | $1-2    | $2-3    |
| Data Transfer       | 15GB Free Tier            | $0      | $0-1    | $1      |
| S3 Backup           | ~5GB storage              | $0      | $0-1    | $1      |
| **TOTAL**           |                           | **$3-5**| **$6-13**| **$13-20**|

### **Per-Client Incremental Cost**

**DynamoDB Storage & Operations (per client/month):**

- **Storage:** ~500MB = $0.125
- **Writes:** 43,200/month = $0.054
- **Reads:** 30,000/month = $0.0075
- **Total per client: ~$0.19/month**

**Example Scaling:**

- **10 clients:** $5 + ($0.19 × 10) = **$6.90/month**
- **20 clients:** $5 + ($0.19 × 20) = **$8.80/month**
- **50 clients:** $5 + ($0.19 × 50) = **$14.50/month**
- **100 clients:** $5 + ($0.19 × 100) = **$24/month**

**Email Costs (AWS SES):**

- **First 62,000 emails/month:** Free (if sent from EC2).
- **Additional:** $0.10 per 1,000 emails.
- **Typical usage:** 2-5 emails per client/month = **Free**.

### **Cost Optimization Strategies**

- DynamoDB TTL for automatic data cleanup.
- Redis caching reduces read costs by 80%.
- Batch writes for efficiency.
- On-demand pricing (no provisioned capacity waste).
- Free Tier maximization (12 months).
- Email batching for cost efficiency.

---

## 8. Risk Assessment

| Risk                     | Impact | Probability | Mitigation                                                 |
| ------------------------ | ------ | ----------- | ---------------------------------------------------------- |
| Budget overrun           | Medium | Low         | AWS Budget alerts, daily monitoring, DynamoDB on-demand.   |
| EC2 downtime             | High   | Low         | CloudWatch alarms, systemd auto-restart, 98% uptime target.|
| Client data security     | High   | Low         | Fernet encryption, IAM least privilege, audit logging.     |
| DynamoDB hot partitions  | Medium | Low         | Proper partition key design, client_id sharding.           |
| Email delivery issues    | Medium | Low         | AWS SES monitoring, retry logic, fallback notifications.   |
| Worker manager failure   | Medium | Low         | Health checks, auto-restart, error logging.                |
| Scope creep              | Medium | High        | Strict MVP definition, feature freeze week 8, Phase 2 planning.|

---

## 9. Expected Outcomes

### **Technical Deliverables**

**Working Multi-Tenant Platform:**

- Client signup with email verification.
- 5 DynamoDB tables with proper data isolation.
- Automated worker management (10-50 clients).
- Real-time monitoring dashboard per client.
- Email notification system (verification + alerts).
- Cost, security, and recommendations features.
- Settings page with email/notification management.

**Performance Targets:**

- **API response time:** <300ms (cached), <2s (uncached).
- **Email delivery:** <30 seconds.
- **Data collection:** Every 5 minutes per client.
- **Platform uptime:** 98-99%.
- **Worker startup:** <10 seconds per client.

**Security & Compliance:**

- Encrypted credential storage.
- Email verification before critical alerts.
- Data isolation between clients.
- IAM least privilege roles.
- Audit logging for all operations.

### **Learning Outcomes**

**AWS Services Mastery:**

- Advanced DynamoDB (multi-tenant, GSI, TTL).
- AWS SES integration (transactional emails).
- CloudWatch, Cost Explorer, GuardDuty.
- IAM roles and encryption (Fernet).

**SaaS Architecture:**

- Multi-tenant data modeling.
- Background worker orchestration.
- Encrypted credential management.
- Email notification system.
- Self-service signup flow.

**Full-Stack Development:**

- FastAPI async programming.
- React state management.
- DynamoDB access patterns.
- Email template design.
- RESTful API design.

### **Portfolio Value**

This project demonstrates:

- **Enterprise-level architecture** (multi-tenant SaaS).
- **Security-first design** (encryption, email verification).
- **AWS expertise** (10+ services integrated).
- **Full-stack skills** (Python, React, DynamoDB, SES).
- **Cost optimization** ($0.19/client/month at scale).
- **Production readiness** (monitoring, logging, error handling).
- **Professional features** (email system, automated alerts).

**Key Differentiators:**

- Not a simple monitoring tool - a complete SaaS platform.
- Demonstrates ability to design for multiple tenants.
- Shows understanding of encryption and security.
- Includes modern SaaS features (email verification, notifications).
- Cost-efficient scaling ($20/month for 50+ clients).

---

## 10. Conclusion

**AWS Cloud Health Dashboard** is a **production-grade multi-tenant SaaS platform** that demonstrates:

1. **Enterprise Architecture Skills**
   - Multi-tenant data modeling.
   - Encrypted credential management.
   - Background worker orchestration.
   - Scalable design (10-100+ clients).

2. **AWS Expertise**
   - Deep integration with 10+ AWS services.
   - Cost-optimized infrastructure design.
   - Security best practices.
   - Email notification system (SES).

3. **Full-Stack Development**
   - Modern tech stack (FastAPI + React).
   - Professional UI/UX.
   - Email template design.
   - RESTful API design.

4. **Business Acumen**
   - Cost-efficient operation ($10-20/month).
   - Scalable pricing model ($0.19/client).
   - Clear ROI for clients (15-25% cost savings).
   - Professional SaaS features.

**Timeline:** 3 months | **Team:** 4 people | **Budget:** $10-20/month

---

## Appendices

**A. GitHub Repository:** [https://github.com/Unvianpetronas/Cloud_health_dashboard](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**B. Contact Information:**

- **Project Lead:** Truong Quoc Tuan
- **Email:** `unviantruong26@gmail.com`
- **WhatsApp:** `+84 798806545`