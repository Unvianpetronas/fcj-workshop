
```markdown
---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 200
chapter: false
pre: " <b> 2. </b> "
---

# AWS Cloud Health Dashboard
## Giải pháp giám sát và tối ưu hóa hạ tầng AWS toàn diện

### 1. Tóm tắt điều hành

AWS Cloud Health Dashboard là nền tảng giám sát và tối ưu hóa hạ tầng AWS được thiết kế nhằm giúp các doanh nghiệp và cá nhân quản lý hiệu quả chi phí, bảo mật, và hiệu suất của hệ thống cloud. Nền tảng sử dụng kiến trúc đơn giản với EC2 và DynamoDB, kết hợp các dịch vụ AWS như CloudWatch, Cost Explorer, Security Hub để cung cấp giám sát thời gian thực và phân tích dữ liệu.

**Điểm nổi bật:**
- Chi phí vận hành: $12-18/tháng (tận dụng Free Tier)
- Giám sát CloudWatch metrics và AWS costs
- Phân tích chi phí và đề xuất tối ưu hóa dựa trên AWS APIs
- Dashboard real-time với data caching
- Kiến trúc đơn giản, dễ maintain và scale

### 2. Vấn đề

**Vấn đề hiện tại:**

Nhiều doanh nghiệp và cá nhân sử dụng AWS đang gặp phải các thách thức:

1. **Chi phí không kiểm soát**: Thiếu công cụ giám sát chi phí tập trung dẫn đến hoá đơn AWS bất ngờ tăng cao
2. **Bảo mật thiếu visibility**: Không có dashboard tổng hợp cho security findings
3. **Metrics phân tán**: Phải chuyển đổi giữa nhiều console AWS khác nhau
4. **Thiếu historical data**: CloudWatch chỉ giữ metrics 15 ngày (free tier)
5. **Không có alerting tùy chỉnh**: Khó setup alerts phức tạp

**Giải pháp:**

Cloud Health Dashboard cung cấp một nền tảng tập trung với các tính năng:

- **Centralized monitoring**: Dashboard duy nhất cho CloudWatch, Cost Explorer, Security Hub
- **Data persistence với DynamoDB**: 
  - 4 bảng chuyên biệt: CloudHealthMetrics, CloudHealthCosts, SecurityFindings, Recommendations
  - TTL tự động để xóa dữ liệu cũ (30 ngày cho metrics, 365 ngày cho costs, 90 ngày cho security, 180 ngày cho recommendations)
  - On-demand pricing để tiết kiệm chi phí
  - Optimized query patterns với GSI cho từng loại data
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
  - Redis caching để giảm AWS API calls
  - Pre-collected data trong DynamoDB
  - WebSocket cho real-time updates

**Lợi ích và ROI:**

1. **Tiết kiệm chi phí**: 
   - Visibility vào unutilized resources
   - AWS native recommendations (Cost Explorer, Trusted Advisor)
   - Chi phí nền tảng chỉ $12-18/tháng
   - Potential savings: 15-25% bằng cách identify waste

2. **Tăng visibility**:
   - Single dashboard thay vì 5+ AWS consoles
   - Historical data retention
   - Custom alerts
   - Real-time notifications

3. **Cải thiện productivity**:
   - Giảm thời gian monitoring từ 30 phút/ngày xuống 5 phút/ngày
   - Automated data collection
   - Proactive alerts
   - Actionable recommendations

### 3. Kiến trúc giải pháp

**Tổng quan kiến trúc:**

Nền tảng sử dụng kiến trúc Single EC2 Instance + DynamoDB để tối ưu chi phí. EC2 instance chạy tất cả application components, trong khi DynamoDB lưu trữ historical data với 4 tables chuyên biệt.

![Architecture Diagram](/images/2-Proposal/project_aws.drawio.png)

**Dịch vụ AWS sử dụng:**

1. **Amazon EC2** (Compute):
   - t3.micro instance (750h/month Free Tier)
   - Single public subnet (no NAT Gateway needed)
   - Components: Nginx, FastAPI, Redis, React, CloudWatch Agent
   - Security: Restrictive security groups, Systems Manager Session Manager

2. **Amazon DynamoDB** (Storage):
   - 4 bảng chuyên biệt: Metrics, Costs, Security, Recommendations
   - On-demand pricing mode
   - TTL enabled cho tự động cleanup
   - Global Secondary Indexes cho query optimization
   - Point-in-time recovery for backups

3. **Amazon CloudWatch**:
   - Metrics collection từ EC2, RDS, S3, Lambda, etc.
   - Custom metrics cho application monitoring
   - Logs aggregation
   - Alarms và notifications

4. **AWS Cost Explorer**:
   - Cost và usage data via API
   - Rightsizing recommendations (AWS native)
   - Cost forecast (AWS native)
   - Service breakdown

5. **AWS Security Hub**:
   - Security findings aggregation
   - Compliance checking
   - Integration với GuardDuty

6. **Amazon GuardDuty** (Optional):
   - Threat detection
   - Anomaly monitoring

7. **Amazon S3** (Backup):
   - DynamoDB backup exports
   - CloudWatch logs archive

**Single Public Subnet Design:**

EC2 instance được đặt trong public subnet với các biện pháp bảo mật:

1. **Security Group Configuration:**
   - Inbound: Port 80, 443 từ 0.0.0.0/0
   - Inbound: Port 22 chỉ từ trusted IPs (hoặc disable)
   - Outbound: Unrestricted (cho AWS API calls)

2. **Access Management:**
   - AWS Systems Manager Session Manager thay cho SSH
   - IAM roles với least privilege principle
   - No hardcoded credentials trong code

3. **Monitoring & Logging:**
   - VPC Flow Logs enabled
   - CloudWatch alarms cho security events
   - GuardDuty threat detection

**Architecture Decision:**
Private subnet architecture được cân nhắc nhưng không implement vì:
- Tăng chi phí 100% ($33/tháng cho NAT Gateway hoặc $14/tháng cho VPC Endpoints)
- Không phù hợp với learning/portfolio project scope
- Public subnet + security best practices đủ cho use case này
- Demonstrate cost-aware decision making

Kiến trúc này phù hợp cho development/demo environment. Production deployment trong enterprise sẽ cần private subnet với NAT Gateway hoặc VPC Endpoints.

**Components trong EC2 Instance:**

1. **Nginx (Port 80/443)**
   - Reverse proxy và web server
   - Serve React static files
   - Proxy API requests tới FastAPI
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

**Thiết kế DynamoDB Tables (4 tables):**

Sử dụng 4 tables chuyên biệt để tối ưu performance và separation of concerns:

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

Tách thành 4 tables chuyên biệt thay vì 2 tables tổng hợp vì:

1. **Query Performance**: Mỗi table optimize cho access pattern riêng
   - Metrics: Time-series queries với high write throughput
   - Costs: Aggregation queries theo service và date range
   - Security: Severity-based filtering và real-time monitoring
   - Recommendations: Impact-based sorting và status tracking

2. **Separation of Concerns**:
   - Rõ ràng về data ownership và lifecycle
   - TTL policies khác nhau cho từng loại data
   - Dễ maintain và troubleshoot

3. **Scalability**:
   - Độc lập scale từng table khi cần
   - Không bị hot partition khi một loại data tăng đột biến
   - GSI optimize cho query patterns cụ thể

4. **Cost Management**:
   - Monitor và optimize cost theo từng table
   - Flexible TTL cho từng use case
   - On-demand billing phù hợp với variable workload

**Trade-offs:**
- Chi phí cao hơn ~$2-3/tháng so với 2 tables
- Complexity cao hơn khi deploy và maintain
- Nhưng: Better performance, cleaner architecture, easier to scale

### 4. Triển khai kỹ thuật

**Stack công nghệ:**

**Backend:**
- Python 3.9+ với FastAPI framework
- boto3 cho AWS SDK
- Redis cho caching
- uvicorn ASGI server
- asyncio cho async operations

**Frontend:**
- React 18 với Vite
- TanStack Query (React Query) cho data fetching
- Recharts cho data visualization
- Tailwind CSS cho styling
- Axios cho HTTP client

**Database:**
- DynamoDB làm primary database (4 tables)
- Redis cho caching (in-memory)

**DevOps:**
- Nginx làm reverse proxy
- Systemd cho service management
- CloudWatch Agent cho monitoring
- Bash scripts cho automation

**Core Features Implementation:**

1. **Metrics Collection Service**
   - Background job chạy mỗi 5 phút
   - Collect metrics từ CloudWatch API
   - Store vào CloudHealthMetrics table
   - Cache trong Redis (5 minute TTL)

2. **Cost Analysis Service**
   - Daily job collect cost data từ Cost Explorer API
   - Store historical data trong CloudHealthCosts table
   - Generate charts và trends
   - Display AWS native recommendations

3. **Security Monitoring**
   - Poll Security Hub và GuardDuty findings
   - Store trong SecurityFindings table
   - Display active findings với severity filtering
   - Alert on high/critical severity
   - Read-only integration

4. **Recommendations Engine**
   - Analyze metrics, costs, và security data
   - Generate actionable recommendations
   - Store trong Recommendations table
   - Prioritize by impact và confidence
   - Track implementation status

5**Caching Strategy**
   - Redis cache cho frequent queries
   - 5-minute TTL cho metrics data
   - 1-hour TTL cho cost data
   - 30-minute TTL cho security findings
   - 2-hour TTL cho recommendations
   - Cache invalidation on data refresh

6**Security Measures**
   - HTTPS only với Let's Encrypt
   - IAM roles với least privilege
   - Security groups restrictive rules
   - No hardcoded credentials
   - VPC Flow Logs enabled

### 5. Lộ trình & Mốc triển khai

**MVP Features (3 tháng, 4 người):**

```
THÁNG 1: Core Infrastructure & Basic Features
├─ Tuần 1: Infrastructure Setup (1 người DevOps + 3 người support)
│  ├─ EC2 instance launch và configuration
│  ├─ DynamoDB tables creation (4 tables với GSIs)
│  ├─ Security groups, IAM roles
│  ├─ Development environment setup
│  └─ Deliverable: Working infrastructure với 4 DynamoDB tables
│
├─ Tuần 2: Backend Foundation (2 người Backend + 2 người Frontend start)
│  ├─ FastAPI skeleton với basic endpoints
│  ├─ DynamoDB connection và basic CRUD cho 4 tables
│  ├─ CloudWatch integration (read metrics)
│  ├─ React app initialization
│  └─ Deliverable: API responding, Frontend routing
│
├─ Tuần 3: Core Monitoring (2 Backend + 2 Frontend)
│  ├─ Metrics collection background job
│  ├─ Store metrics in CloudHealthMetrics table
│  ├─ Basic dashboard page với charts
│  ├─ Display real CloudWatch data
│  └─ Deliverable: Metrics monitoring working
│
└─ Tuần 4: Cost Integration (2 Backend + 2 Frontend)
   ├─ Cost Explorer API integration
   ├─ Cost data storage in CloudHealthCosts table
   ├─ Cost dashboard page
   ├─ Charts và trends visualization
   └─ Deliverable: Month 1 MVP - Metrics + Costs monitoring

THÁNG 2: Additional Features & Polish
├─ Tuần 5: Redis Caching (1 Backend + 1 DevOps + 2 Frontend)
│  ├─ Redis setup và integration
│  ├─ Cache layer implementation cho 4 tables
│  ├─ Performance optimization
│  ├─ Frontend improvements
│  └─ Deliverable: Improved performance
│
├─ Tuần 6: Security Monitoring (2 Backend + 2 Frontend)
│  ├─ Security Hub và GuardDuty integration
│  ├─ Store findings in SecurityFindings table
│  ├─ Security dashboard page với severity filtering
│  ├─ Alert system basic
│  └─ Deliverable: Security visibility
│
├─ Tuần 7: Recommendations Engine (2 Backend + 2 Frontend)
│  ├─ AWS Cost Explorer recommendations integration
│  ├─ Rule-based cost optimization logic
│  ├─ Store in Recommendations table
│  ├─ Recommendations UI với impact sorting
│  ├─ Testing và bug fixes
│  └─ Deliverable: Optimization suggestions
│
└─ Tuần 8: Integration & Testing (All 4)
   ├─ End-to-end testing across 4 tables
   ├─ Bug fixes
   ├─ Performance tuning
   ├─ Documentation
   └─ Deliverable: Stable application

THÁNG 3: Production Ready & Deployment
├─ Tuần 9: Deployment Automation (1 DevOps + 3 support)
│  ├─ SSL certificates setup
│  ├─ Nginx configuration
│  ├─ Systemd services
│  ├─ Monitoring setup
│  └─ Deliverable: Production deployment
│
├─ Tuần 10: Documentation & Polish (All 4)
│  ├─ User documentation
│  ├─ API documentation
│  ├─ DynamoDB schema documentation
│  ├─ Deployment guide
│  ├─ UI/UX improvements
│  └─ Deliverable: Production-ready system
│
├─ Tuần 11: Testing & Bug Fixes (All 4)
│  ├─ Load testing
│  ├─ Security testing
│  ├─ DynamoDB performance testing
│  ├─ Bug fixing
│  ├─ Performance optimization
│  └─ Deliverable: Stable production
│
└─ Tuần 12: Demo Preparation (All 4)
   ├─ Demo environment setup
   ├─ Presentation materials
   ├─ Final testing
   ├─ Knowledge transfer
   └─ Deliverable: Project handover

Post-deployment: Maintenance & Enhancements
└─ Monitoring, bug fixes, feature requests
```

**Phase 2 (Future Enhancements - 3-6 tháng sau):**
- Machine Learning cost prediction models
- Multi-account support
- Advanced analytics
- Well-Architected Framework assessment
- GuardDuty deep integration
- AWS Config compliance tracking
- Custom alerting workflows
- Mobile responsive improvements
- Advanced recommendation algorithms

### 6. Ước tính ngân sách

**Chi phí hạ tầng AWS (Monthly):**

| Dịch vụ | Mô tả | Tháng 1 | Tháng 2 | Tháng 3 |
|---------|-------|---------|---------|---------|
| **EC2 t3.micro** | 750h Free Tier | $0 | $0 | $0 |
| **DynamoDB** | On-demand, 4 tables | $3-4 | $4-6 | $6-8 |
| **CloudWatch** | Metrics + Logs (Free Tier) | $0-1 | $1-2 | $2-3 |
| **Data Transfer** | 15GB Free Tier | $0 | $0-1 | $1 |
| **S3** | Backup storage (~5GB) | $0 | $0-1 | $1 |
| **Security Services** | GuardDuty (optional) | $0 | $1 | $1-2 |
| **TỔNG** | | **$3-5** | **$6-11** | **$11-18** |

**Chi phí trung bình 12 tháng: $12-18/tháng**

**DynamoDB Cost Breakdown (4 tables):**
- **Lưu trữ**: ~5GB total = $1.25/tháng
   - CloudHealthMetrics: 2GB ($0.50)
   - CloudHealthCosts: 1GB ($0.25)
   - SecurityFindings: 1GB ($0.25)
   - Recommendations: 1GB ($0.25)
- **Writes**: ~800K requests/tháng = $1.00/tháng
   - Metrics: 500K writes ($0.625)
   - Costs: 100K writes ($0.125)
   - Security: 100K writes ($0.125)
   - Recommendations: 100K writes ($0.125)
- **Reads**: ~4M requests/tháng = $2.00/tháng
   - Distributed across 4 tables
- **GSI**: Included in on-demand pricing
- **Backup**: Free (Point-in-time recovery)
- **Tổng DynamoDB**: $4-7/tháng

**Note:** Chi phí có thể tăng nếu:
- Thu thập metrics frequency cao hơn
- Nhiều AWS services được monitor
- Retention period dài hơn
- GuardDuty enabled ($4-6/tháng thêm)
- Nhiều security findings và recommendations

**Cost Optimization Strategies:**
- Sử dụng Free Tier tối đa (12 tháng)
- DynamoDB on-demand thay vì provisioned
- TTL tự động xóa data cũ cho từng table
- Redis caching giảm DynamoDB reads
- CloudWatch log retention 7 ngày
- Disable unused AWS services monitoring
- Batch writes khi possible
- Efficient query patterns với GSI

### 7. Đánh giá rủi ro

**Ma trận rủi ro:**

| Rủi ro | Mức độ ảnh hưởng | Xác suất | Mức độ rủi ro |
|--------|------------------|----------|---------------|
| Chi phí vượt ngân sách | Trung bình | Trung bình | Trung bình |
| EC2 downtime | Cao | Thấp | Trung bình |
| Scope creep | Trung bình | Cao | Cao |
| Technical complexity | Trung bình | Trung bình | Trung bình |
| AWS API rate limits | Thấp | Trung bình | Thấp |
| DynamoDB hot partitions | Thấp | Thấp | Thấp |
| Team coordination | Trung bình | Trung bình | Trung bình |

**Chiến lược giảm thiểu:**

1. **Chi phí**:
   - AWS Budget alerts tại $15, $20
   - Daily cost monitoring trong Cost Explorer
   - DynamoDB on-demand (không bị surprise bills)
   - Monitor DynamoDB costs per table
   - Weekly cost review meetings
   - Kill switch để disable data collection nếu vượt budget

2. **EC2 Availability**:
   - CloudWatch alarms cho health checks
   - Systemd auto-restart services
   - Health check endpoint
   - Backup và recovery procedures documented
   - Target: 98-99% uptime (realistic cho single instance)

3. **Scope Creep**:
   - Strict MVP definition
   - Feature freeze sau week 8
   - "Nice to have" list cho Phase 2
   - Weekly standup để track progress
   - Prioritize must-have features
   - Document 4-table design clearly

4. **Technical Complexity**:
   - Start với simplest solution
   - Extensive use of AWS documentation
   - Code review process
   - Technical spikes cho unknown areas
   - DynamoDB data modeling workshops
   - Mentor consultation when stuck

5. **API Rate Limits**:
   - Implement exponential backoff
   - Cache aggressively với Redis
   - Monitor API usage
   - Stay within free tier limits
   - Batch operations when possible

6. **DynamoDB Hot Partitions**:
   - Proper partition key design
   - Write sharding cho high-traffic tables
   - Monitor CloudWatch metrics cho throttling
   - Use GSI efficiently

7. **Team Coordination**:
   - Daily standups (15 minutes)
   - Clear task assignments
   - Git workflow (feature branches, PRs)
   - Documentation from day 1
   - Shared knowledge base
   - DynamoDB schema documentation

**Kế hoạch dự phòng:**

1. **Service Failure**: Systemd auto-restart, documented manual recovery
2. **Data Loss**: Daily S3 backups, DynamoDB PITR enabled
3. **Cost Spike**: Immediate notification, manual review, throttle data collection
4. **Behind Schedule**: Cut Phase 2 features, focus on MVP only
5. **DynamoDB Issues**: Fallback to direct API calls, reduce write frequency

### 8. Kết quả kỳ vọng

**Technical Deliverables:**

1. **Working Dashboard:**
   - 4 main pages: Metrics, Costs, Security, Recommendations
   - Real data từ AWS services
   - Responsive design
   - Basic authentication

2. **Data Collection System:**
   - Background jobs collecting metrics mỗi 5 phút
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
   - Real-time alerts cho critical findings
   - Compliance status overview

6. **Recommendations System:**
   - Cost optimization suggestions
   - Performance improvement recommendations
   - Security enhancement suggestions
   - Impact-based prioritization using GSI
   - Implementation tracking

**Learning Outcomes:**

1. **AWS Services:**
   - Hands-on experience với EC2, DynamoDB (advanced), CloudWatch
   - Cost Explorer API integration
   - Security Hub và GuardDuty understanding
   - IAM roles và policies
   - VPC networking basics
   - DynamoDB data modeling best practices

2. **Full-Stack Development:**
   - FastAPI backend development
   - React frontend development
   - RESTful API design
   - NoSQL database design patterns
   - DynamoDB access patterns và GSI optimization
   - Caching strategies

3. **DevOps:**
   - Linux server administration
   - Nginx configuration
   - Service management với systemd
   - SSL/TLS setup
   - Monitoring và logging
   - Database backup strategies

4. **Best Practices:**
   - Security-first design
   - Cost optimization
   - Code organization
   - Documentation
   - Testing strategies
   - NoSQL data modeling

**Portfolio Value:**

Project này demonstrate:
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
- Single instance (không high availability)
- Public subnet (không private network isolation)
- Rule-based recommendations (không ML-powered initially)
- Manual AWS service additions
- Limited to AWS (không multi-cloud)

Documented trade-offs:
- 4 tables tăng complexity và cost ($2-3/tháng) nhưng cải thiện performance và maintainability
- Public subnet giảm security nhưng tiết kiệm $33/tháng
- Single instance giảm availability nhưng phù hợp với budget constraint

Những limitations và trade-offs này là conscious decisions cho cost efficiency và project timeline, demonstrating real-world engineering judgment.

---

**A. Links:**
- [GitHub Repository](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**B. Liên hệ:**
- Project Lead: Trương Quốc Tuấn
- Email: unviantruong26@gmail.com
- WhatsApp: 0798806545
```

---

