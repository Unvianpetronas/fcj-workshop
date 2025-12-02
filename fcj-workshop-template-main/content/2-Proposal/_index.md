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

AWS Cloud Health Dashboard is a production-grade, multi-tenant SaaS platform with automated DevSecOps implementation that enables businesses to monitor and optimize AWS infrastructure for multiple clients from a single centralized system.

### Key Highlights:

- **Production-Grade Architecture:** Application Load Balancer with SSL/TLS termination, Elastic IP, health checks, and zero-downtime deployment capability
- **DevSecOps Pipeline:** Automated CI/CD with security scanning and blue-green deployment strategy
- **Multi-tenant Architecture:** Designed to support 10-50+ AWS client accounts (demo tested with 3-5 clients)
- **High Availability:** ALB with health checks, automatic failover capability, and 99.99% SLA
- **Platform cost:** $39-49/month (Year 1) with production-grade infrastructure
- **Per-client cost:** ~$0.59/month (scalable and predictable)
- **Security-first design:** AWS Secrets Manager + KMS encryption + ACM certificates
- **Automated deployment:** CodePipeline + CodeBuild with zero-downtime deployment
- **5 DynamoDB tables:** Optimized data model with client isolation
- **Redis caching:** Potential 60-80% database read cost reduction
- **Email notification system:** AWS SES integration for critical alerts
- **Elastic IP:** FREE static IP for production stability
- **Technology Stack:** FastAPI (Python) + React + DynamoDB + Redis + ALB + Elastic IP + ACM + AWS Secrets Manager + KMS + CloudWatch + CloudTrail + CodePipeline + CodeBuild + EC2 t3.micro

---

## 2. Problem Statement

### Current Challenges:

Businesses managing AWS infrastructure for multiple clients face:

- **No centralized monitoring:** Must log into each client's AWS console separately
- **Scattered security alerts:** Critical GuardDuty findings go unnoticed
- **Cost visibility gaps:** Difficult to track and optimize costs across clients
- **Insecure credential management:** AWS access keys stored in databases or config files
- **No automated deployment:** Manual deployment leads to errors and downtime
- **Lack of security scanning:** Vulnerabilities deployed to production
- **No audit logging:** Cannot track security events and API calls
- **Manual monitoring:** 30+ minutes daily per client
- **Downtime during deployments:** No zero-downtime deployment strategy
- **Dynamic IPs:** IP changes on restart causing DNS/whitelist issues

### Our Solution:

Cloud Health Dashboard provides a production-grade platform with DevSecOps practices including:

#### Production-Grade Infrastructure

- Application Load Balancer for high availability and traffic distribution
- Elastic IP for stable, persistent IP address (FREE when attached)
- SSL/TLS termination with AWS Certificate Manager (ACM)
- Health checks with automatic failover
- Zero-downtime deployment capability
- Horizontal scaling readiness (currently single EC2, designed for multi-instance)
- 99.99% ALB SLA guarantee

#### DevSecOps Architecture

- Automated CI/CD pipeline (CodePipeline + CodeBuild)
- Security scanning (SAST with Bandit, dependency scanning with Safety)
- Blue-green deployment strategy for zero downtime
- Infrastructure monitoring with CloudWatch
- Audit logging with CloudTrail
- Secrets management with AWS Secrets Manager + KMS

#### Multi-Tenant Architecture

- Scalable architecture to monitor 10-50+ AWS client accounts (MVP demo with 3-5 clients)
- AWS Secrets Manager for credential storage (KMS encrypted)
- Complete data isolation between clients
- Task scheduling system for data collection

#### Centralized Monitoring

- Single dashboard for all clients
- Real-time infrastructure health
- Historical data retention (30-365 days)
- 15+ AWS services monitored
- Redis caching for performance optimization

---

## 3. Solution Architecture

### Architecture Overview

![final_architecture.drawio.png](/images/2-Proposal/final_architecture.drawio.png)

### Key Components:

- **Internet Gateway:** VPC entry point for incoming traffic
- **Application Load Balancer (ALB):**
   - Distributes traffic across availability zones
   - SSL/TLS termination with ACM certificates
   - Health checks with automatic failover
   - Path-based routing capability
   - DDoS protection (AWS Shield Standard)
- **Elastic IP:**
   - Static public IPv4 address for EC2 instance
   - FREE when attached to running instance
   - Persistent across instance restarts
   - Enables stable SSH access and IP whitelisting
- **EC2 Instance (t3.micro):** Application server processing
- **Zero-Downtime Deployment:** Blue-green strategy using target groups
- **DynamoDB:** Multi-tenant data storage
- **Redis:** In-memory caching layer
- **AWS Secrets Manager:** Secure credential storage
- **CloudWatch & CloudTrail:** Monitoring and audit logging

### Data Flow

#### 1. DEVELOPER WORKFLOW (DevSecOps with Zero-Downtime Deployment)

```
Developer → Git push → GitHub → CodePipeline triggered
→ CodeBuild runs:
  • Install dependencies
  • Run tests (pytest)
  • Security scan (Bandit, Safety)
  • Build frontend
  • SSH to EC2 (via Elastic IP) → Deploy new version
  • ALB health check validates deployment
  • Traffic cutover (zero downtime)
→ CloudWatch logs everything
```

#### 2. INCOMING TRAFFIC (Production Flow)

```
Client Browser → HTTPS (Cloudflare CDN/SSL)
→ Internet Gateway → ALB (ACM certificate, SSL termination)
→ Target Group (health check: /health endpoint)
→ EC2 Instance (Elastic IP attached, FastAPI application)
→ Response back through ALB → Client
```

#### 3. CLIENT SIGNUP

```
Customer → Enter AWS Access key → Click login → Account auto create
→ API (through ALB) → Validate AWS keys
→ Store in Secrets Manager (KMS encrypted)
→ Store metadata in DynamoDB
```

#### 4. EMAIL VERIFICATION

```
Customer → Enter email → Check email → Send verify email 
→ Click link → ALB → API → Verify token
→ Mark email verified → CloudTrail logs event
```

#### 5. WORKER MANAGER (Multi-Tenant)

```
Every 15 min → Fetch active clients from DynamoDB
→ For each client:
  • Get credentials from Secrets Manager
  • Schedule async task for data collection
  • Collect AWS data
  • Cache in Redis (5 min TTL)
  • Store in DynamoDB
```

#### 6. DATA COLLECTION (Per Client)

```
Async Task → Secrets Manager (get credentials)
→ Client AWS API → Collect metrics
→ Cache in Redis → Store in DynamoDB (partitioned by aws_account_id)
→ If critical finding → Send email alert (SES)
→ CloudWatch logs all operations
```

#### 7. DASHBOARD DISPLAY

```
Customer login → Request through ALB → FastAPI checks Redis cache
→ Cache HIT: Return cached data (< 200ms)
→ Cache MISS: Query DynamoDB (filtered by aws_account_id)
→ Store in Redis → Return data through ALB
```

#### 8. HEALTH CHECKS & FAILOVER

```
ALB → Every 30s → GET /health endpoint
→ If unhealthy (2 consecutive failures):
  • Mark target unhealthy
  • Stop routing traffic
  • CloudWatch alarm triggered
  • Systemd attempts auto-restart
→ When healthy: Resume traffic routing
```

#### 9. SSH ACCESS & MANAGEMENT

```
DevOps Engineer → SSH to Elastic IP
→ Direct EC2 access for:
  • Deployment verification
  • Log inspection
  • Emergency troubleshooting
  • System maintenance
→ Stable IP (no DNS updates needed)
```

#### 10. SECURITY & AUDIT

```
All API calls → CloudTrail logging
Credentials access → CloudWatch alarm
Failed auth attempts → Email alert
KMS key usage → Audit trail
ALB access logs → S3 (optional, for compliance)
```

---

## 4. Key Features

### Production-Grade Infrastructure

#### Application Load Balancer (ALB):

- **High Availability:** Distributes traffic across multiple availability zones
- **SSL/TLS Termination:** ACM certificates for HTTPS encryption
- **Health Checks:** Automatic failover if EC2 becomes unhealthy
- **Path-Based Routing:** Frontend and API routing (future expansion ready)
- **DDoS Protection:** AWS Shield Standard (automatic, no cost)
- **99.99% SLA:** AWS-guaranteed uptime
- **Zero-Downtime Deployment:** Target group switching for blue-green deployments

#### Elastic IP:

- **Static IP Address:** Persistent public IPv4 that never changes
- **FREE Cost:** $0/month when attached to running EC2 instance
- **Stable SSH Access:** Reliable management access with fixed IP
- **IP Whitelisting:** Enable third-party integrations with consistent IP
- **No DNS Updates:** IP persists across instance restarts
- **Disaster Recovery:** Quick failover while maintaining same IP

#### SSL/TLS Security:

- ACM-managed certificates (automatic renewal)
- TLS 1.2/1.3 support
- End-to-end encryption (Cloudflare → ALB → EC2)
- Perfect Forward Secrecy (PFS)
- Strong cipher suites only

#### Scalability Readiness:

- Horizontal scaling capable (currently single EC2)
- Target group architecture supports multiple instances
- Auto Scaling Group ready (future implementation)
- Connection draining for graceful shutdowns

### DevSecOps Implementation

#### Automated CI/CD Pipeline:

- GitHub integration for source control
- CodePipeline for orchestration (1 pipeline FREE)
- CodeBuild for automated builds (100 mins/month FREE)
- Automated security scanning before deployment
- Zero-downtime deployment via ALB target groups
- SSH deployment to Elastic IP for reliability
- Health check validation before traffic cutover

#### Security Scanning:

- **Bandit:** Static Application Security Testing (SAST)
- **Safety:** Dependency vulnerability scanning
- Pre-deployment security gates
- Build fails on HIGH severity issues

#### Deployment Strategy:

- Blue-green deployment using ALB target groups
- Health check validation (/health endpoint)
- Automatic rollback on failure
- Connection draining during deployment
- Zero customer impact during updates

#### Secrets Management:

- AWS Secrets Manager for credential storage
- KMS encryption for all secrets
- Rotation policy support (configurable in production)
- No credentials in code or environment variables
- IAM role-based access only

#### Monitoring & Logging:

- CloudWatch for application logs and metrics
- CloudTrail for API audit logging
- ALB access logs (optional, for compliance)
- Custom metrics for business KPIs
- Automated alerts for anomalies
- 90-day log retention

### Infrastructure Security:

#### Network Layer:

- **Security Groups:** HTTPS (443) from ALB only to EC2
- **ALB Security Group:** HTTPS (443) from Internet (0.0.0.0/0)
- **SSH access (22)** to Elastic IP from management IPs only

#### Application Layer:

- JWT-based authentication with IP validation
- Rate limiting (100 req/min per IP)
- CORS policy enforcement

#### Infrastructure Layer:

- AWS Shield Standard (DDoS protection)
- SSL/TLS termination at ALB
- IAM roles with least privilege
- KMS encryption for data at rest
- VPC isolation

### Multi-Tenant Management

- Self-service client signup with AWS key validation
- Credentials stored in Secrets Manager (KMS encrypted)
- Task scheduling system for periodic data collection
- Complete data isolation
- Per-client dashboard access
- Redis caching for performance optimization

### Email Notification System

- Email verification with secure tokens (24h expiry)
- Critical alerts via AWS SES
- Customizable notification preferences
- HTML email templates
- Email management in Settings page
- **Note:** SES sandbox mode requires email verification before sending

### Infrastructure Monitoring

- Real-time metrics from 15+ AWS services
- Historical data retention (30+ days)
- Redis caching (performance optimization)
- EC2, S3, RDS, Lambda monitoring
- Service health dashboards
- ALB metrics (request count, latency, healthy targets)

### Cost Optimization

- Daily cost tracking per service
- Monthly cost trends
- AWS Cost Explorer recommendations
- Budget alerts
- Per-client cost analysis

### Security Monitoring

- GuardDuty integration support (demo uses simulated findings)
- Severity-based filtering
- Email alerts for critical findings
- CloudTrail audit logging
- Compliance status tracking

---

## 5. Technical Implementation

### Technology Stack

#### Frontend:

- React 18 with Vite
- TanStack Query for data fetching
- Recharts for visualization
- Tailwind CSS for styling

#### Backend:

- Python 3.12+ with FastAPI
- boto3 (AWS SDK)
- asyncio for concurrent task execution
- Redis for caching
- pytest for testing
- uvicorn ASGI server

#### Infrastructure:

- **Load Balancer:** Application Load Balancer (ALB)
- **Networking:** Elastic IP (static public IPv4)
- **Compute:** EC2 t3.micro (1 vCPU, 1GB RAM, Ubuntu 22.04 LTS)
- **Database:** DynamoDB (5 tables, on-demand pricing)
- **Cache:** Redis (in-memory, localhost)
- **Networking:** VPC with public subnet, Internet Gateway

#### DevSecOps:

- GitHub (source control)
- CodePipeline (CI/CD orchestration)
- CodeBuild (automated builds)
- Bandit (SAST security scanner)
- Safety (dependency scanner)

#### Security:

- AWS Certificate Manager (ACM) - SSL/TLS certificates
- AWS Secrets Manager (credential storage)
- AWS KMS (encryption keys)
- CloudTrail (audit logging)
- CloudWatch (monitoring & alerts)
- Security Groups (network firewall)

#### Email:

- AWS SES (Simple Email Service)
- HTML email templates
- Email verification with tokens
- Alert notifications

---

## 6. Cost Analysis

**Cost Note:** Estimates based on AWS us-east-1 pricing (December 2025) using AWS Pricing Calculator. Assumptions: 10 clients, 15-min collection interval, 30-day data retention.

### Year 1 Costs (Maximum Free Tier)

| AWS Service | Cost/Month | Notes |
|------------|-----------|-------|
| Application Load Balancer | $16.20 | $0.0225/hour + LCU charges |
| Elastic IP | $0 | FREE when attached to running EC2 |
| EC2 t3.micro (750h free) | $0 (12 months) | Free tier first year |
| DynamoDB (25GB free) | $0-3 | Free tier + overage |
| S3 (5GB free) | $0-1 | Backups + ALB logs (optional) |
| CloudWatch (10 metrics free) | $0-2 | Additional metrics |
| Secrets Manager (1 secret) | $0.40 | Per secret/month |
| ACM Certificate | $0 | FREE for ALB/CloudFront |
| CodePipeline (1 pipeline free) | $0 | First pipeline free |
| CodeBuild (100 mins/mo free) | $0 | Within free tier |
| CloudTrail (1 trail free) | $0 | Management events free |
| KMS (20,000 requests free) | $0-1 | Mostly within free tier |
| SES (3,000 emails free) | $0 | Verification + alerts |
| Data Transfer (1GB free) | $0-1 | ALB to EC2 (no charge in same AZ) |
| **TOTAL Year 1** | **$39-49/month** | Production-grade infrastructure |

### Year 2+ Costs

| AWS Service | Cost/Month | Notes |
|------------|-----------|-------|
| Application Load Balancer | $16.20 | $0.0225/hour + LCU charges |
| Elastic IP | $0 | FREE when attached to running EC2 |
| EC2 t3.micro | $8-10 | On-demand pricing |
| DynamoDB | $3-5 | Storage + operations |
| CloudWatch | $2-4 | Logs + metrics |
| Secrets Manager | $0.40 | Per secret/month |
| ACM Certificate | $0 | FREE for ALB |
| CodePipeline | $0 | First pipeline free |
| CodeBuild | $0 | Within free tier limits |
| CloudTrail | $0 | Management events free |
| KMS | $1-2 | Key storage + requests |
| SES | $2-5 | Email sending |
| S3 | $1 | Backups + logs |
| Data Transfer | $1 | Outbound traffic |
| **TOTAL Year 2+** | **$47-57/month** | Production-grade with ALB |

### Scaling Costs

#### Year 1 (Free Tier EC2):

- **10 clients:** Platform $39 + (10 × $0.59) = $44.90/month
- **20 clients:** Platform $39 + (20 × $0.59) = $50.80/month
- **50 clients:** Platform $39 + (50 × $0.59) = $68.50/month

#### Year 2+ (Paid EC2):

- **10 clients:** Platform $47 + (10 × $0.59) = $52.90/month
- **20 clients:** Platform $47 + (20 × $0.59) = $58.80/month
- **50 clients:** Platform $47 + (50 × $0.59) = $76.50/month

**Scale Note:** With EC2 t3.micro, the platform can efficiently support 10-20 clients. To scale to 50+ clients, upgrade to larger instances (t3.small/medium) is recommended. ALB architecture makes horizontal scaling straightforward by adding instances to the target group.

### Future Optimizations:

- **Reserved Instances:** 30-40% savings on EC2 (after 12 months)
- **Savings Plans:** Flexible commitment-based discounts
- **Spot Instances:** For non-critical background tasks (70-90% savings)
- **S3 Intelligent-Tiering:** Automatic cost optimization for backups
- **CloudWatch Logs optimization:** Adjust retention (7-90 days)

### Total Infrastructure Investment:

- **Year 1:** $39-49/month (with free tier EC2 + Elastic IP)
- **Year 2+:** $47-57/month (full pricing, Elastic IP still FREE)
- **Per Client:** $0.59/month (scalable and predictable)

This represents exceptional value for a production-grade, highly available, secure multi-tenant SaaS platform with static IP infrastructure.

---

## 7. Risk Assessment

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|-----------|
| Budget overrun | Medium | Low | AWS Budget alerts, Redis caching, DynamoDB on-demand, ALB cost monitoring, Elastic IP kept attached (FREE) |
| ALB costs exceed estimates | Medium | Low | Monitor LCU usage, optimize for single AZ (dev), CloudWatch cost alarms |
| Elastic IP charges | Low | Very Low | Keep EC2 running 24/7, monitor attachment status, CloudWatch alarm if detached |
| EC2 downtime | High | Very Low | ALB health checks, systemd auto-restart, automatic failover, Elastic IP enables quick recovery, target 99.5% uptime |
| ALB misconfiguration | High | Low | Health check validation, target group testing, CodeBuild deployment validation |
| SSL certificate expiry | Medium | Very Low | ACM auto-renewal (60 days before expiry), CloudWatch alarms |
| IP address changes | Low | Very Low | Elastic IP prevents IP changes, persistent across restarts |
| SSH access loss | Medium | Very Low | Elastic IP provides stable SSH endpoint, backup access via ALB |
| Client data security | High | Low | Secrets Manager + KMS, IAM least privilege, audit logging, ALB access logs |
| CI/CD pipeline failure | Medium | Low | CodeBuild retry logic, deployment rollback, health check gates, SSH via Elastic IP for manual fixes |
| Redis cache failure | Medium | Low | Systemd monitor, auto-restart, graceful degradation to DynamoDB |
| DynamoDB hot partitions | Medium | Low | Proper partition key design, aws_account_id sharding |
| Secrets Manager costs | Medium | Medium | Monitor usage, evaluate shared secret patterns in future |
| Email delivery issues | Medium | Low | AWS SES monitoring, retry logic, fallback notifications |
| Security vulnerability in deps | High | Medium | Safety scanner in CI/CD, automated updates, security alerts |
| Scope creep | Medium | High | Strict MVP definition, feature freeze week 8, Phase 2 plan |
| Health check false positives | Low | Low | Tune health check thresholds (2 failures before unhealthy) |

---

## 8. Expected Outcomes

### Technical Deliverables

#### Production-Grade Infrastructure:

- Application Load Balancer with SSL/TLS termination
- Elastic IP for stable SSH access and IP whitelisting (FREE)
- ACM-managed certificates with automatic renewal
- Health checks with automatic failover capability
- Zero-downtime deployment using target groups
- Horizontal scaling readiness (multi-instance capable)
- 99.99% ALB uptime SLA

#### DevSecOps Platform:

- Automated CI/CD pipeline (GitHub → CodePipeline → CodeBuild → EC2 via Elastic IP)
- Automated security scanning (Bandit SAST, Safety dependency scan)
- Blue-green deployment strategy through ALB
- CloudWatch monitoring and CloudTrail audit logging
- AWS Secrets Manager for credential storage
- KMS encryption for all sensitive data
- Redis caching layer on EC2

#### Multi-Tenant SaaS:

- Client signup with email verification
- 5 DynamoDB tables with proper data isolation
- Task scheduling system for data collection (demo with 3-5 clients)
- Real-time monitoring dashboard per client
- Email notification system
- Settings page with email/notification management

#### Performance Targets:

- **API response time:** Target <200ms (Redis cache HIT), <2s (cache MISS)
- **ALB response time:** Additional <50ms latency (SSL termination + routing)
- **SSH access:** Direct access via Elastic IP for deployment and troubleshooting
- **Email delivery:** <30 seconds (SES sandbox mode, requires email verification)
- **Data collection:** Every 15 minutes per client
- **Platform uptime:** Target 99.5%+ (ALB 99.99% SLA + EC2 with health checks)
- **Health check interval:** 30 seconds (2 consecutive failures = unhealthy)
- **Deployment time:** <5 minutes (zero downtime with blue-green)
- **Build time:** Target <5 minutes (within free tier limits)

#### Security & Compliance:

- Zero credentials in code or environment variables
- All secrets encrypted with KMS
- SSL/TLS end-to-end encryption (Cloudflare → ALB → EC2)
- Stable Elastic IP for SSH access (no dynamic IP changes)
- Email verification before critical alerts
- Complete API audit trail (CloudTrail)
- ALB access logs available (optional, for compliance)
- Data isolation between clients
- IAM least privilege roles
- Automated security scanning in CI/CD

### Learning Outcomes

#### Production Infrastructure Mastery:

- Application Load Balancer configuration and management
- Elastic IP allocation and management (cost optimization)
- SSL/TLS certificate management with ACM
- Health checks and target group configuration
- Zero-downtime deployment strategies
- High availability architecture patterns
- Network security with Security Groups
- Static IP management for production stability

#### DevSecOps Mastery:

- CI/CD pipeline design and implementation
- Automated security scanning (SAST, dependency scanning)
- Blue-green deployment implementation
- Secrets management best practices
- Audit logging and compliance
- Monitoring and alerting
- Automated deployment strategies
- SSH-based deployment via stable IP

#### AWS Services Mastery:

- Advanced DynamoDB (multi-tenant, GSI, TTL)
- Application Load Balancer (ALB) configuration
- Elastic IP allocation and best practices
- AWS Certificate Manager (ACM) integration
- AWS Secrets Manager + KMS integration
- CloudWatch + CloudTrail
- CodePipeline + CodeBuild
- AWS SES integration
- IAM roles and policies
- VPC networking and Security Groups

#### SaaS Architecture:

- Multi-tenant data modeling
- Background task orchestration
- Secure credential management
- Caching strategies (Redis)
- Email notification system
- Scalable infrastructure design
- Static IP infrastructure patterns

#### Full-Stack Development:

- FastAPI async programming
- React state management
- Redis integration
- Security-first development
- RESTful API design
- Health check endpoint implementation

### Portfolio Value

This project demonstrates:

#### Production-Grade Infrastructure

- Application Load Balancer for high availability
- Elastic IP for stable infrastructure (FREE optimization)
- SSL/TLS termination with ACM certificates
- Zero-downtime deployment capability
- Health checks with automatic failover
- Horizontal scaling readiness

#### DevSecOps Implementation

- Automated CI/CD pipeline with security scanning
- Blue-green deployment strategy
- Secrets management with AWS Secrets Manager + KMS
- Comprehensive monitoring and logging
- Automated deployment with health validation
- SSH deployment via static Elastic IP

#### Enterprise Architecture

- Scalable multi-tenant SaaS design
- Infrastructure scalable (MVP tested with 3-5, designed for 10-50+ clients)
- Redis caching layer
- Background task scheduling system
- Production-ready security patterns
- Cost-optimized infrastructure (Elastic IP FREE)

#### Security Expertise

- Zero secrets in code
- End-to-end encryption (Cloudflare → ALB → EC2)
- KMS encryption
- CloudTrail audit logging
- Automated vulnerability scanning
- Multi-layer security (network + application + infrastructure)
- Stable IP for secure SSH access

#### AWS Proficiency

- 17+ AWS services integrated (added ALB + Elastic IP + ACM)
- Cost-optimized design (Elastic IP FREE when attached)
- Free tier maximization
- IAM best practices
- High availability architecture
- Static IP management

#### Cost Efficiency

- $39-49/month for platform (Year 1)
- $0.59/client/month incremental
- Redis caching for read cost optimization
- Elastic IP FREE (saves $3.65/month per IP)
- Justifiable ALB cost for production-grade features

### Key Differentiators:

- Production-grade infrastructure with ALB, Elastic IP, and ACM (not just basic EC2)
- Zero-downtime deployment capability (blue-green strategy)
- 99.99% uptime SLA through ALB
- Stable IP infrastructure with FREE Elastic IP
- Cost optimization (Elastic IP attached = $0)
- Automated DevSecOps pipeline with security gates
- Production-grade security (Secrets Manager + KMS + ACM)
- Automated CI/CD with automated testing
- Multi-tenant architecture with data isolation
- Comprehensive monitoring and audit logging
- Horizontal scaling readiness (demonstrates understanding of growth)

---

## 9. Conclusion

AWS Cloud Health Dashboard is a production-grade, multi-tenant SaaS platform with automated DevSecOps practices that demonstrates:

### Production Infrastructure

- Application Load Balancer for high availability and traffic management
- Elastic IP for stable, persistent IP address (FREE when attached)
- SSL/TLS termination with AWS Certificate Manager
- Health checks with automatic failover
- Zero-downtime deployment capability
- Horizontal scaling readiness
- 99.99% ALB uptime SLA

### DevSecOps Implementation

- Automated CI/CD pipeline (CodePipeline + CodeBuild)
- Security scanning before every deployment
- Blue-green deployment strategy
- SSH deployment via stable Elastic IP
- Secrets management with AWS Secrets Manager + KMS
- Comprehensive monitoring (CloudWatch + CloudTrail)

### Enterprise Architecture

- Scalable multi-tenant design (MVP tested with 3-5, designed for 10-50+ clients)
- Scalable task scheduling system
- Redis caching for performance
- Complete data isolation
- Production-ready infrastructure patterns
- Cost-optimized design (Elastic IP FREE)

### Security-First Design

- Zero secrets in code or config files
- End-to-end encryption (Cloudflare → ALB → EC2)
- KMS encryption for all sensitive data
- CloudTrail audit logging
- Automated vulnerability scanning
- Multi-layer security architecture
- Stable SSH access via Elastic IP

### AWS Expertise

- Deep integration with 17+ AWS services
- Cost-optimized infrastructure ($39-49/month Year 1)
- Elastic IP cost optimization (FREE vs $3.65/month)
- Security best practices
- Email notification system (SES)
- High availability architecture

### Business Acumen

- Cost-efficient operation with production-grade features
- Scalable pricing model ($0.59/client/month)
- Professional SaaS features
- Clear ROI for clients
- Justifiable infrastructure investment

### Investment Justification:

The ~$16/month for ALB + $0/month for Elastic IP (FREE) is strategically justified:

- Transforms project from demo to production-grade portfolio piece
- Demonstrates enterprise architecture patterns
- Enables zero-downtime deployments
- Provides 99.99% uptime SLA
- Shows scalability readiness (critical for DevOps/SRE roles)
- Proves understanding of high availability and failover strategies
- Elastic IP adds production stability at zero cost
- Shows cost optimization skills (keeping Elastic IP attached = FREE)

**Timeline:** 3 months | **Team:** 4 people | **Budget:** $39-49/month (Year 1), $47-57/month (Year 2+)

This project demonstrates production-grade infrastructure and DevSecOps practices implementation, making it a compelling portfolio piece for cloud engineering, DevOps, and SRE roles.

---

## Appendices

### A. Services Used (17 AWS Services - Production Grade):

1. **Application Load Balancer (ALB)** - Traffic distribution & high availability
2. **Elastic IP** - Static public IPv4 address (FREE when attached to running EC2)
3. **AWS Certificate Manager (ACM)** - SSL/TLS certificate management (FREE for ALB)
4. **EC2** (Compute)
5. **DynamoDB** (Database)
6. **AWS Secrets Manager** (Credential storage)
7. **AWS KMS** (Encryption)
8. **CloudWatch** (Monitoring)
9. **CloudTrail** (Audit logging)
10. **CodePipeline** (CI/CD)
11. **CodeBuild** (Automated builds)
12. **SES** (Email)
13. **S3** (Backup & ALB logs)
14. **VPC** (Networking)
15. **Internet Gateway** (Connectivity)
16. **Security Groups** (Firewall)
17. **IAM** (Access management)
18. **Shield Standard** (DDoS protection - automatic)

### B. Architecture Improvements from ALB + Elastic IP:

- **High Availability:** 99.99% uptime SLA
- **Zero-Downtime Deployment:** Blue-green strategy with target groups
- **SSL/TLS Termination:** Offload encryption from EC2
- **Health Checks:** Automatic failover capability
- **Horizontal Scaling:** Ready to add multiple EC2 instances
- **DDoS Protection:** AWS Shield Standard integration
- **Stable IP Infrastructure:** Elastic IP (FREE when attached)
- **SSH Management:** Reliable access via fixed Elastic IP
- **IP Whitelisting:** Enable third-party integrations
- **Cost Optimization:** Elastic IP FREE (saves $3.65/month)
- **Production-Grade:** Enterprise-ready infrastructure pattern

### C. Cost Optimization Wins:

- **Elastic IP:** $0/month (FREE when attached) vs $3.65/month (idle)
- **ACM Certificate:** $0/month (FREE for ALB) vs $0.75/month (public certificate)
- **Redis Caching:** 60-80% reduction in DynamoDB read costs
- **Free Tier:** EC2, CodePipeline, CodeBuild, S3, CloudWatch (Year 1)
- **Efficient Architecture:** Single ALB + Elastic IP serves all clients

### D. GitHub Repository

https://github.com/Unvianpetronas/Cloud_health_dashboard

### E. Domain

cloudhealthdashboard.xyz (Cloudflare DNS + CDN)

### F. Contact Information

- **Project Lead:** Truong Quoc Tuan
- **Email:** unviantruong26@gmail.com
- **WhatsApp:** +84 798806545

---