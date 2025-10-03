---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 200
chapter: false
pre: " <b> 2. </b> "
---

# AWS Cloud Health Dashboard
## Comprehensive AWS Infrastructure Monitoring and Optimization Solution

### 1. Executive Summary

AWS Cloud Health Dashboard is a monitoring and optimization platform designed to help businesses and individuals effectively manage the cost, security, and performance of their cloud systems. The platform uses a simple architecture with EC2 and DynamoDB, combined with AWS services such as CloudWatch, Cost Explorer, and Security Hub to provide real-time monitoring and data analysis.

**Key Highlights:**
- Operating cost: $12-18/month (leveraging Free Tier)
- CloudWatch metrics and AWS cost monitoring
- Cost analysis and optimization recommendations based on AWS APIs
- Real-time dashboard with data caching
- Simple architecture, easy to maintain and scale

### 2. Problem Statement

**Current Problems:**

Many businesses and individuals using AWS face the following challenges:

1. **Uncontrolled costs**: Lack of centralized cost monitoring tools leads to unexpectedly high AWS bills
2. **Security visibility gaps**: No consolidated dashboard for security findings
3. **Scattered metrics**: Must switch between multiple AWS consoles
4. **Lack of historical data**: CloudWatch only retains metrics for 15 days (free tier)
5. **No custom alerting**: Difficult to set up complex alerts

**Solution:**

Cloud Health Dashboard provides a centralized platform with the following features:

- **Centralized monitoring**: Single dashboard for CloudWatch, Cost Explorer, Security Hub
- **Data persistence with DynamoDB**: 
  - 4 specialized tables: CloudHealthMetrics, CloudHealthCosts, SecurityFindings, Recommendations
  - Automatic TTL to delete old data (30 days for metrics, 365 days for costs, 90 days for security, 180 days for recommendations)
  - On-demand pricing for cost savings
  - Optimized query patterns with GSI for each data type
- **Cost analysis**: 
  - Historical cost trends
  - Service breakdown
  - AWS Cost Explorer recommendations integration
  - Budget alerts
- **Security monitoring**: 
  - Security Hub findings aggregation
  - GuardDuty threat detection display
  - Compliance status tracking
  - Severity-based filtering
- **Intelligent recommendations**:
  - Cost optimization suggestions
  - Performance improvements
  - Security enhancements
  - Impact-based prioritization
- **Performance**: 
  - Redis caching to reduce AWS API calls
  - Pre-collected data in DynamoDB
  - WebSocket for real-time updates

**Benefits and ROI:**

1. **Cost savings**: 
   - Visibility into unutilized resources
   - AWS native recommendations (Cost Explorer, Trusted Advisor)
   - Platform cost only $12-18/month
   - Potential savings: 15-25% by identifying waste

2. **Increased visibility**:
   - Single dashboard instead of 5+ AWS consoles
   - Historical data retention
   - Custom alerts
   - Real-time notifications

3. **Improved productivity**:
   - Reduce monitoring time from 30 minutes/day to 5 minutes/day
   - Automated data collection
   - Proactive alerts
   - Actionable recommendations

### 3. Solution Architecture

**Architecture Overview:**

The platform uses a Single EC2 Instance + DynamoDB architecture to optimize costs. The EC2 instance runs all application components, while DynamoDB stores historical data with 4 specialized tables.

![Architecture Diagram](/images/2-Proposal/project_aws.drawio.png)

**AWS Services Used:**

1. **Amazon EC2** (Compute):
   - t3.micro instance (750h/month Free Tier)
   - Single public subnet (no NAT Gateway needed)
   - Components: Nginx, FastAPI, Redis, React, CloudWatch Agent
   - Security: Restrictive security groups, Systems Manager Session Manager

2. **Amazon DynamoDB** (Storage):
   - 4 specialized tables: Metrics, Costs, Security, Recommendations
   - On-demand pricing mode
   - TTL enabled for automatic cleanup
   - Global Secondary Indexes for query optimization
   - Point-in-time recovery for backups

3. **Amazon CloudWatch**:
   - Metrics collection from EC2, RDS, S3, Lambda, etc.
   - Custom metrics for application monitoring
   - Logs aggregation
   - Alarms and notifications

4. **AWS Cost Explorer**:
   - Cost and usage data via API
   - Rightsizing recommendations (AWS native)
   - Cost forecast (AWS native)
   - Service breakdown

5. **AWS Security Hub**:
   - Security findings aggregation
   - Compliance checking
   - Integration with GuardDuty

6. **Amazon GuardDuty** (Optional):
   - Threat detection
   - Anomaly monitoring

7. **Amazon S3** (Backup):
   - DynamoDB backup exports
   - CloudWatch logs archive

**Single Public Subnet Design:**

EC2 instance is placed in a public subnet with security measures:

1. **Security Group Configuration:**
   - Inbound: Port 80, 443 from 0.0.0.0/0
   - Inbound: Port 22 only from trusted IPs (or disabled)
   - Outbound: Unrestricted (for AWS API calls)

2. **Access Management:**
   - AWS Systems Manager Session Manager instead of SSH
   - IAM roles with least privilege principle
   - No hardcoded credentials in code

3. **Monitoring & Logging:**
   - VPC Flow Logs enabled
   - CloudWatch alarms for security events
   - GuardDuty threat detection

**Architecture Decision:**
Private subnet architecture was considered but not implemented because:
- Increases cost by 100% ($33/month for NAT Gateway or $14/month for VPC Endpoints)
- Not suitable for learning/portfolio project scope
- Public subnet + security best practices sufficient for this use case
- Demonstrates cost-aware decision making

This architecture is suitable for development/demo environments. Production deployment in enterprise would require private subnet with NAT Gateway or VPC Endpoints.

**Components in EC2 Instance:**

1. **Nginx (Port 80/443)**
   - Reverse proxy and web server
   - Serve React static files
   - Proxy API requests to FastAPI
   - SSL/TLS termination
   - Gzip compression

2. **FastAPI (Port 8000)**
   - RESTful API backend
   - AWS SDK integration (boto3)
   - Business logic processing
   - Authentication/Authorization
   - Background task scheduling

3. **Redis (Port 6379)**
   - Cache AWS API responses
   - Cache DynamoDB query results
   - Session storage
   - Rate limiting
   - Typical cache hit rate: 60-80%

4. **React Frontend**
   - Single Page Application (SPA)
   - Dashboard visualization
   - API client
   - Real-time updates via polling/WebSocket
   - Responsive design

5. **CloudWatch Agent**
   - EC2 metrics collection
   - Application logs shipping
   - Custom metrics publishing

**DynamoDB Tables Design (4 tables):**

Using 4 specialized tables to optimize performance and separation of concerns:

**Table 1: CloudHealthMetrics**
```python
{
    "TableName": "CloudHealthMetrics",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # service#metric_name
        {"AttributeName": "sk", "KeyType": "RANGE"}   # ISO timestamp
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "MetricNameIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # metric_name
                {"AttributeName": "sk", "KeyType": "RANGE"}        # timestamp
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 30 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "EC2#CPUUtilization",
    "sk": "2025-09-22T14:30:00Z",
    "gsi1_pk": "CPUUtilization",  # For cross-service queries
    "value": 75.5,
    "unit": "Percent",
    "service": "EC2",
    "metric_name": "CPUUtilization",
    "dimensions": {"InstanceId": "i-1234567890abcdef0"},
    "ttl": 1669824000  # Unix timestamp
}
```

**Table 2: CloudHealthCosts**
```python
{
    "TableName": "CloudHealthCosts",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # service
        {"AttributeName": "sk", "KeyType": "RANGE"}   # date#granularity
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"}
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 365 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "EC2",
    "sk": "2025-09-22#DAILY",
    "date": "2025-09-22",
    "granularity": "DAILY",
    "cost": 12.45,
    "usage_quantity": 24.0,
    "usage_unit": "Hrs",
    "currency": "USD",
    "tags": {"Environment": "Production"},
    "ttl": 1704067200  # Unix timestamp (1 year)
}
```

**Table 3: SecurityFindings**
```python
{
    "TableName": "SecurityFindings",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # finding_type
        {"AttributeName": "sk", "KeyType": "RANGE"}   # finding_id
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"},
        {"AttributeName": "gsi1_sk", "AttributeType": "S"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "SeverityIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # severity
                {"AttributeName": "gsi1_sk", "KeyType": "RANGE"}  # created_at
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 90 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "GuardDuty",
    "sk": "finding-abc123def456",
    "gsi1_pk": "HIGH",  # For severity-based queries
    "gsi1_sk": "2025-09-22T14:30:00Z",
    "finding_id": "finding-abc123def456",
    "finding_type": "GuardDuty",
    "severity": "HIGH",
    "status": "ACTIVE",
    "title": "Suspicious network traffic detected",
    "description": "Unusual outbound traffic from EC2 instance to known malicious IP",
    "service": "EC2",
    "resource_id": "i-1234567890abcdef0",
    "resource_type": "Instance",
    "recommendation": "Review security group rules and investigate instance activity",
    "created_at": "2025-09-22T14:30:00Z",
    "updated_at": "2025-09-22T14:30:00Z",
    "ttl": 1677484800  # Unix timestamp (90 days)
}
```

**Table 4: Recommendations**
```python
{
    "TableName": "Recommendations",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # type
        {"AttributeName": "sk", "KeyType": "RANGE"}   # created_at#rec_id
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"},
        {"AttributeName": "gsi1_sk", "AttributeType": "N"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "ImpactIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # impact#service
                {"AttributeName": "gsi1_sk", "KeyType": "RANGE"}  # estimated_savings
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 180 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "cost",
    "sk": "2025-09-22T14:30:00Z#rec-789xyz",
    "gsi1_pk": "HIGH#EC2",  # For impact-based queries
    "gsi1_sk": 50.00,  # estimated_savings for sorting
    "rec_id": "rec-789xyz",
    "type": "cost",
    "title": "Rightsize EC2 instance i-1234567890abcdef0",
    "description": "Instance runs at <10% CPU utilization. Consider downsizing from t3.large to t3.small",
    "impact": "HIGH",
    "effort": "LOW",
    "confidence": 0.92,
    "estimated_savings": 50.00,
    "estimated_savings_period": "monthly",
    "service": "EC2",
    "resource_id": "i-1234567890abcdef0",
    "resource_type": "Instance",
    "current_config": "t3.large",
    "recommended_config": "t3.small",
    "action_steps": [
        "1. Create AMI backup",
        "2. Stop instance",
        "3. Change instance type",
        "4. Restart and monitor"
    ],
    "implemented": false,
    "created_at": "2025-09-22T14:30:00Z",
    "updated_at": "2025-09-22T14:30:00Z",
    "ttl": 1685894400  # Unix timestamp (180 days)
}
```

**Design Rationale:**

Splitting into 4 specialized tables instead of 2 consolidated tables because:

1. **Query Performance**: Each table optimized for specific access patterns
  - Metrics: Time-series queries with high write throughput
  - Costs: Aggregation queries by service and date range
  - Security: Severity-based filtering and real-time monitoring
  - Recommendations: Impact-based sorting and status tracking

2. **Separation of Concerns**:
  - Clear data ownership and lifecycle
  - Different TTL policies for each data type
  - Easier to maintain and troubleshoot

3. **Scalability**:
  - Independently scale each table when needed
  - Avoid hot partitions when one data type spikes
  - GSI optimized for specific query patterns

4. **Cost Management**:
  - Monitor and optimize cost per table
  - Flexible TTL for each use case
  - On-demand billing suitable for variable workload

**Trade-offs:**
- Higher cost ~$2-3/month compared to 2 tables
- Higher complexity when deploying and maintaining
- But: Better performance, cleaner architecture, easier to scale

### 4. Technical Implementation

**Technology Stack:**

**Backend:**
- Python 3.9+ with FastAPI framework
- boto3 for AWS SDK
- Redis for caching
- uvicorn ASGI server
- asyncio for async operations

**Frontend:**
- React 18 with Vite
- TanStack Query (React Query) for data fetching
- Recharts for data visualization
- Tailwind CSS for styling
- Axios for HTTP client

**Database:**
- DynamoDB as primary database (4 tables)
- Redis for caching (in-memory)

**DevOps:**
- Nginx as reverse proxy
- Systemd for service management
- CloudWatch Agent for monitoring
- Bash scripts for automation

**Core Features Implementation:**

1. **Metrics Collection Service**
  - Background job running every 5 minutes
  - Collect metrics from CloudWatch API
  - Store in CloudHealthMetrics table
  - Cache in Redis (5 minute TTL)

2. **Cost Analysis Service**
  - Daily job collects cost data from Cost Explorer API
  - Store historical data in CloudHealthCosts table
  - Generate charts and trends
  - Display AWS native recommendations

3. **Security Monitoring**
  - Poll Security Hub and GuardDuty findings
  - Store in SecurityFindings table
  - Display active findings with severity filtering
  - Alert on high/critical severity
  - Read-only integration

4. **Recommendations Engine**
  - Analyze metrics, costs, and security data
  - Generate actionable recommendations
  - Store in Recommendations table
  - Prioritize by impact and confidence
  - Track implementation status

5. **Caching Strategy**
  - Redis cache for frequent queries
  - 5-minute TTL for metrics data
  - 1-hour TTL for cost data
  - 30-minute TTL for security findings
  - 2-hour TTL for recommendations
  - Cache invalidation on data refresh

6. **Security Measures**
  - HTTPS only with Let's Encrypt
  - IAM roles with least privilege
  - Security groups restrictive rules
  - No hardcoded credentials
  - VPC Flow Logs enabled

### 5. Development Roadmap & Milestones

**MVP Features (3 months, 4 people):**

```
MONTH 1: Core Infrastructure & Basic Features
├─ Week 1: Infrastructure Setup (1 DevOps + 3 support)
│  ├─ EC2 instance launch and configuration
│  ├─ DynamoDB tables creation (4 tables with GSIs)
│  ├─ Security groups, IAM roles
│  ├─ Development environment setup
│  └─ Deliverable: Working infrastructure with 4 DynamoDB tables
│
├─ Week 2: Backend Foundation (2 Backend + 2 Frontend start)
│  ├─ FastAPI skeleton with basic endpoints
│  ├─ DynamoDB connection and basic CRUD for 4 tables
│  ├─ CloudWatch integration (read metrics)
│  ├─ React app initialization
│  └─ Deliverable: API responding, Frontend routing
│
├─ Week 3: Core Monitoring (2 Backend + 2 Frontend)
│  ├─ Metrics collection background job
│  ├─ Store metrics in CloudHealthMetrics table
│  ├─ Basic dashboard page with charts
│  ├─ Display real CloudWatch data
│  └─ Deliverable: Metrics monitoring working
│
└─ Week 4: Cost Integration (2 Backend + 2 Frontend)
   ├─ Cost Explorer API integration
   ├─ Cost data storage in CloudHealthCosts table
   ├─ Cost dashboard page
   ├─ Charts and trends visualization
   └─ Deliverable: Month 1 MVP - Metrics + Costs monitoring

MONTH 2: Additional Features & Polish
├─ Week 5: Redis Caching (1 Backend + 1 DevOps + 2 Frontend)
│  ├─ Redis setup and integration
│  ├─ Cache layer implementation for 4 tables
│  ├─ Performance optimization
│  ├─ Frontend improvements
│  └─ Deliverable: Improved performance
│
├─ Week 6: Security Monitoring (2 Backend + 2 Frontend)
│  ├─ Security Hub and GuardDuty integration
│  ├─ Store findings in SecurityFindings table
│  ├─ Security dashboard page with severity filtering
│  ├─ Basic alert system
│  └─ Deliverable: Security visibility
│
├─ Week 7: Recommendations Engine (2 Backend + 2 Frontend)
│  ├─ AWS Cost Explorer recommendations integration
│  ├─ Rule-based cost optimization logic
│  ├─ Store in Recommendations table
│  ├─ Recommendations UI with impact sorting
│  ├─ Testing and bug fixes
│  └─ Deliverable: Optimization suggestions
│
└─ Week 8: Integration & Testing (All 4)
   ├─ End-to-end testing across 4 tables
   ├─ Bug fixes
   ├─ Performance tuning
   ├─ Documentation
   └─ Deliverable: Stable application

MONTH 3: Production Ready & Deployment
├─ Week 9: Deployment Automation (1 DevOps + 3 support)
│  ├─ SSL certificates setup
│  ├─ Nginx configuration
│  ├─ Systemd services
│  ├─ Monitoring setup
│  └─ Deliverable: Production deployment
│
├─ Week 10: Documentation & Polish (All 4)
│  ├─ User documentation
│  ├─ API documentation
│  ├─ DynamoDB schema documentation
│  ├─ Deployment guide
│  ├─ UI/UX improvements
│  └─ Deliverable: Production-ready system
│
├─ Week 11: Testing & Bug Fixes (All 4)
│  ├─ Load testing
│  ├─ Security testing
│  ├─ DynamoDB performance testing
│  ├─ Bug fixing
│  ├─ Performance optimization
│  └─ Deliverable: Stable production
│
└─ Week 12: Demo Preparation (All 4)
   ├─ Demo environment setup
   ├─ Presentation materials
   ├─ Final testing
   ├─ Knowledge transfer
   └─ Deliverable: Project handover

Post-deployment: Maintenance & Enhancements
└─ Monitoring, bug fixes, feature requests
```

**Phase 2 (Future Enhancements - 3-6 months later):**
- Machine Learning cost prediction models
- Multi-account support
- Advanced analytics
- Well-Architected Framework assessment
- GuardDuty deep integration
- AWS Config compliance tracking
- Custom alerting workflows
- Mobile responsive improvements
- Advanced recommendation algorithms

### 6. Budget Estimation

**AWS Infrastructure Costs (Monthly):**

| Service | Description | Month 1 | Month 2 | Month 3 |
|---------|-------------|---------|---------|---------|
| **EC2 t3.micro** | 750h Free Tier | $0 | $0 | $0 |
| **DynamoDB** | On-demand, 4 tables | $3-4 | $4-6 | $6-8 |
| **CloudWatch** | Metrics + Logs (Free Tier) | $0-1 | $1-2 | $2-3 |
| **Data Transfer** | 15GB Free Tier | $0 | $0-1 | $1 |
| **S3** | Backup storage (~5GB) | $0 | $0-1 | $1 |
| **Security Services** | GuardDuty (optional) | $0 | $1 | $1-2 |
| **TOTAL** | | **$3-5** | **$6-11** | **$11-18** |

**Average 12-month cost: $12-18/month**

**DynamoDB Cost Breakdown (4 tables):**
- **Storage**: ~5GB total = $1.25/month
  - CloudHealthMetrics: 2GB ($0.50)
  - CloudHealthCosts: 1GB ($0.25)
  - SecurityFindings: 1GB ($0.25)
  - Recommendations: 1GB ($0.25)
- **Writes**: ~800K requests/month = $1.00/month
  - Metrics: 500K writes ($0.625)
  - Costs: 100K writes ($0.125)
  - Security: 100K writes ($0.125)
  - Recommendations: 100K writes ($0.125)
- **Reads**: ~4M requests/month = $2.00/month
  - Distributed across 4 tables
- **GSI**: Included in on-demand pricing
- **Backup**: Free (Point-in-time recovery)
- **Total DynamoDB**: $4-7/month

**Note:** Costs may increase if:
- Higher metrics collection frequency
- More AWS services monitored
- Longer retention period
- GuardDuty enabled ($4-6/month additional)
- More security findings and recommendations

**Cost Optimization Strategies:**
- Maximize Free Tier usage (12 months)
- DynamoDB on-demand instead of provisioned
- Automatic TTL data deletion for each table
- Redis caching reduces DynamoDB reads
- CloudWatch log retention 7 days
- Disable unused AWS services monitoring
- Batch writes when possible
- Efficient query patterns with GSI

### 7. Risk Assessment

**Risk Matrix:**

| Risk | Impact Level | Probability | Risk Level |
|------|--------------|-------------|------------|
| Budget overrun | Medium | Medium | Medium |
| EC2 downtime | High | Low | Medium |
| Scope creep | Medium | High | High |
| Technical complexity | Medium | Medium | Medium |
| AWS API rate limits | Low | Medium | Low |
| DynamoDB hot partitions | Low | Low | Low |
| Team coordination | Medium | Medium | Medium |

**Mitigation Strategies:**

1. **Costs**:
  - AWS Budget alerts at $15, $20
  - Daily cost monitoring in Cost Explorer
  - DynamoDB on-demand (no surprise bills)
  - Monitor DynamoDB costs per table
  - Weekly cost review meetings
  - Kill switch to disable data collection if over budget

2. **EC2 Availability**:
  - CloudWatch alarms for health checks
  - Systemd auto-restart services
  - Health check endpoint
  - Documented backup and recovery procedures
  - Target: 98-99% uptime (realistic for single instance)

3. **Scope Creep**:
  - Strict MVP definition
  - Feature freeze after week 8
  - "Nice to have" list for Phase 2
  - Weekly standup to track progress
  - Prioritize must-have features
  - Document 4-table design clearly

4. **Technical Complexity**:
  - Start with simplest solution
  - Extensive use of AWS documentation
  - Code review process
  - Technical spikes for unknown areas
  - DynamoDB data modeling workshops
  - Mentor consultation when stuck

5. **API Rate Limits**:
  - Implement exponential backoff
  - Cache aggressively with Redis
  - Monitor API usage
  - Stay within free tier limits
  - Batch operations when possible

6. **DynamoDB Hot Partitions**:
  - Proper partition key design
  - Write sharding for high-traffic tables
  - Monitor CloudWatch metrics for throttling
  - Use GSI efficiently

7. **Team Coordination**:
  - Daily standups (15 minutes)
  - Clear task assignments
  - Git workflow (feature branches, PRs)
  - Documentation from day 1
  - Shared knowledge base
  - DynamoDB schema documentation

**Contingency Plans:**

1. **Service Failure**: Systemd auto-restart, documented manual recovery
2. **Data Loss**: Daily S3 backups, DynamoDB PITR enabled
3. **Cost Spike**: Immediate notification, manual review, throttle data collection
4. **Behind Schedule**: Cut Phase 2 features, focus on MVP only
5. **DynamoDB Issues**: Fallback to direct API calls, reduce write frequency

### 8. Expected Outcomes

**Technical Deliverables:**

1. **Working Dashboard:**
  - 4 main pages: Metrics, Costs, Security, Recommendations
  - Real data from AWS services
  - Responsive design
  - Basic authentication

2. **Data Collection System:**
  - Background jobs collecting metrics every 5 minutes
  - Cost data updated daily
  - Security findings updated hourly
  - Recommendations generated daily
  - Data stored in 4 specialized DynamoDB tables

3. **Performance:**
  - Cached response time: < 300ms (60-80% requests)
  - Uncached response time: < 2 seconds
  - DynamoDB query time: 10-25ms (typical)
  - GSI query time: 15-30ms
  - Target uptime: 98-99% (single instance)

4. **Cost Analysis:**
  - Historical cost trends
  - Service breakdown charts
  - AWS Cost Explorer recommendations display
  - Budget alerts
  - Cost forecasting

5. **Security Visibility:**
  - Security Hub findings dashboard
  - GuardDuty threat detection
  - Severity-based filtering using GSI
  - Real-time alerts for critical findings
  - Compliance status overview

6. **Recommendations System:**
  - Cost optimization suggestions
  - Performance improvement recommendations
  - Security enhancement suggestions
  - Impact-based prioritization using GSI
  - Implementation tracking

**Learning Outcomes:**

1. **AWS Services:**
  - Hands-on experience with EC2, DynamoDB (advanced), CloudWatch
  - Cost Explorer API integration
  - Security Hub and GuardDuty understanding
  - IAM roles and policies
  - VPC networking basics
  - DynamoDB data modeling best practices

2. **Full-Stack Development:**
  - FastAPI backend development
  - React frontend development
  - RESTful API design
  - NoSQL database design patterns
  - DynamoDB access patterns and GSI optimization
  - Caching strategies

3. **DevOps:**
  - Linux server administration
  - Nginx configuration
  - Service management with systemd
  - SSL/TLS setup
  - Monitoring and logging
  - Database backup strategies

4. **Best Practices:**
  - Security-first design
  - Cost optimization
  - Code organization
  - Documentation
  - Testing strategies
  - NoSQL data modeling

**Portfolio Value:**

This project demonstrates:
- Real AWS production experience
- Advanced DynamoDB data modeling (4 tables, GSI)
- Full-stack development skills
- Cost-aware architecture decisions
- Security consciousness
- Realistic project scoping
- Team collaboration
- Professional documentation

**Limitations & Trade-offs:**

Documented limitations:
- Single instance (no high availability)
- Public subnet (no private network isolation)
- Rule-based recommendations (not ML-powered initially)
- Manual AWS service additions
- Limited to AWS (not multi-cloud)

Documented trade-offs:
- 4 tables increase complexity and cost ($2-3/month) but improve performance and maintainability
- Public subnet reduces security but saves $33/month
- Single instance reduces availability but fits budget constraint

These limitations and trade-offs are conscious decisions for cost efficiency and project timeline, demonstrating real-world engineering judgment.

---

**A. Links:**
- [GitHub Repository](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**B. Contact:**
- Project Lead: Truong Quoc Tuan
- Email: unviantruong26@gmail.com
- WhatsApp: 0798806545
```

