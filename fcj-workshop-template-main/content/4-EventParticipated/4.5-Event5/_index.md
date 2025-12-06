---
title: "Event 5"
date: "`r Sys.Date()`"
weight: 405
chapter: false
pre: " <b> 4.5. </b> "
---


# AWS Well-Architected Security Pillar Event Report

---

## Executive Summary

I attended a comprehensive half-day workshop on the AWS Well-Architected Security Pillar at the AWS Vietnam Office. The session provided deep technical insights into the five core security pillars: Identity & Access Management, Detection, Infrastructure Protection, Data Protection, and Incident Response. The content was particularly valuable for my Cloud Health Dashboard project, offering practical guidance on implementing production-grade security practices across multi-account AWS environments.

---

## Opening Session: Security Foundation (8:30 - 8:50 AM)

The workshop began with foundational security principles that directly apply to my dashboard architecture. The instructors emphasized three core tenets: **Least Privilege**, **Zero Trust**, and **Defense in Depth**. These aren't just theoretical concepts—they represent the practical security posture that production SaaS applications must maintain.

The discussion of the **Shared Responsibility Model** was particularly relevant. AWS handles security "of" the cloud (physical infrastructure, hypervisor, networking), while we're responsible for security "in" the cloud (application code, data, access controls, configuration). For my Cloud Health Dashboard, this means I own the security of my FastAPI application, JWT authentication implementation, DynamoDB data protection, and IAM role configurations—even though AWS secures the underlying services.

The session highlighted top security threats in Vietnamese cloud environments, including credential leakage through GitHub commits, overly permissive IAM policies, unencrypted data at rest, and inadequate logging. This resonated with my current implementation, where I've already addressed some issues (JWT tokens, encrypted secrets) but have opportunities to strengthen others (comprehensive logging, stricter IAM policies).

---

## Pillar 1: Identity & Access Management (8:50 - 9:30 AM)

The IAM deep dive challenged several assumptions I had about credential management. The key principle emphasized repeatedly was **avoid long-term credentials at all costs**. Static access keys stored in environment variables or configuration files represent a significant attack vector. Instead, the recommended approach uses IAM roles with temporary credentials that rotate automatically.

For my Cloud Health Dashboard, this has immediate implications:

**Current State vs. Best Practice:**
- Currently, my EC2 instance likely uses long-term credentials or instance profiles. The session demonstrated how IAM roles attached to EC2 instances automatically provide temporary credentials that rotate every few hours without any application code changes.
- The background workers that scan client accounts should use **cross-account IAM roles with assume-role** rather than storing separate credentials for each client account. This provides better auditability and automatic credential rotation.

**IAM Identity Center & SSO:**
The introduction to IAM Identity Center (formerly AWS SSO) was eye-opening for multi-account architectures. While my current dashboard isn't quite at the scale where this is necessary, the pattern is clear: as I scale to more client accounts, IAM Identity Center would provide centralized user management with permission sets that define what users can do across all accounts. This eliminates the need to manage IAM users separately in each account.

**Service Control Policies (SCPs) & Permission Boundaries:**
SCPs act as guardrails at the organization level, preventing even administrators from performing dangerous actions like disabling CloudTrail or deleting KMS keys. Permission boundaries provide another layer by setting the maximum permissions that can be granted through IAM policies. For my multi-tenant dashboard, permission boundaries would ensure that even if a client's IAM credentials were compromised, they couldn't exceed predefined limits.

**Practical Tools Demonstrated:**
- **IAM Access Analyzer:** Identifies resources shared with external entities and generates least-privilege policies based on CloudTrail activity. I should integrate this into my dashboard to show clients which of their resources have overly permissive access.
- **IAM Policy Simulator:** Allows testing whether a specific action would be allowed before implementing it. This would be invaluable during development to validate that my cross-account roles have exactly the permissions needed—no more, no less.

**Key Takeaway for My Project:**
Implement cross-account IAM roles with assume-role for client account access, attach IAM roles to my EC2 instance rather than using static credentials, and integrate IAM Access Analyzer findings into my dashboard alongside GuardDuty findings.

---

## Pillar 2: Detection & Continuous Monitoring (9:30 - 9:55 AM)

This session validated several architectural decisions I've already made while highlighting critical gaps in my current implementation.

**CloudTrail at Organization Level:**
The emphasis on organization-level CloudTrail was significant. Instead of enabling CloudTrail separately in each client account, organization-level trails capture API activity across all accounts in a single location. This provides comprehensive audit logs without per-account configuration. For my Cloud Health Dashboard, this means I should recommend that clients enable organization CloudTrail and grant my dashboard read access to those logs, rather than trying to piece together activity from multiple sources.

**GuardDuty - Beyond Basic Findings:**
I'm already integrating GuardDuty findings into my dashboard, but the session revealed I'm only scratching the surface. GuardDuty should be enabled with:
- **S3 Protection:** Monitors S3 data access patterns for anomalies
- **EKS Protection:** For clients running Kubernetes
- **RDS Protection:** Detects suspicious database login attempts
- **Malware Protection:** Scans EBS volumes for malicious software

My current implementation captures basic findings but doesn't categorize by severity or provide contextual recommendations for remediation. The dashboard should prioritize high-severity findings and suggest specific remediation steps.

**Security Hub - Centralized Security Posture:**
Security Hub aggregates findings from GuardDuty, IAM Access Analyzer, Macie, Inspector, and other services into a unified view. More importantly, it runs continuous compliance checks against standards like CIS AWS Foundations Benchmark and PCI-DSS. I should integrate Security Hub into my dashboard as a comprehensive security score rather than just showing raw GuardDuty findings.

**Logging at Every Layer:**
The "logging at every layer" principle was stressed repeatedly:
- **VPC Flow Logs:** Capture network traffic metadata (who's talking to whom)
- **ALB Access Logs:** HTTP request details, response codes, latencies
- **S3 Access Logs:** Every object access, modification, deletion
- **CloudWatch Logs:** Application logs, system logs, custom metrics

For my dashboard, I'm currently missing VPC Flow Logs analysis entirely. This would provide visibility into network-level threats like port scanning, unusual data transfers, or connections to known malicious IPs.

**EventBridge for Automated Response:**
The most practical part was the demonstration of EventBridge rules that trigger Lambda functions in response to security events. For example, when GuardDuty detects a compromised EC2 instance, an EventBridge rule can automatically trigger a Lambda function that:
1. Takes a snapshot of the instance for forensic analysis
2. Isolates the instance by moving it to a quarantine security group
3. Notifies the security team via SNS
4. Creates a ticket in the incident response system

This pattern of "detection → automated response" is far superior to just alerting humans and waiting for manual intervention.

**Key Takeaway for My Project:**
Expand dashboard to include Security Hub aggregated findings, add VPC Flow Logs analysis, implement severity-based prioritization of findings, and build automated response playbooks using EventBridge + Lambda for common scenarios (compromised keys, public S3 buckets, etc.).

---

## Pillar 3: Infrastructure Protection (10:10 - 10:40 AM)

This session directly addressed my current production architecture and upcoming migration plans.

**VPC Segmentation:**
The core principle is workload isolation through subnets. The recommended pattern is:
- **Public subnets:** Only for resources that must accept internet traffic (ALBs, NAT gateways, bastion hosts)
- **Private subnets:** Application tier (EC2, ECS, Lambda)
- **Isolated subnets:** Data tier (RDS, ElastiCache, internal services) with no route to internet

My current Cloud Health Dashboard runs in a public subnet, which is suboptimal. During my planned migration to ECS Fargate, I should restructure to place:
- Application Load Balancer in public subnet
- ECS tasks in private subnet with NAT gateway for outbound access
- DynamoDB and Redis (if moved to ElastiCache) accessible only from private subnet

**Security Groups vs. NACLs:**
The session clarified when to use each:
- **Security Groups:** Stateful, operate at instance level, evaluate all rules before deciding, support allow rules only
- **NACLs:** Stateless, operate at subnet level, process rules in order, support both allow and deny rules

The best practice is to use security groups as the primary control (whitelisting specific ports/protocols/sources) and NACLs as a secondary defense layer to explicitly deny known bad actors or suspicious IP ranges.

For my dashboard:
- Current security group allows SSH (port 22) from my IP, HTTPS (443) from Cloudflare, Redis (6379) from localhost only
- Should add NACL rules to explicitly deny known malicious IP ranges
- Should implement security group rule for backend → DynamoDB using VPC endpoints (no internet routing)

**WAF + Shield + Network Firewall:**
Three layers of network protection were explained:

**AWS WAF (Web Application Firewall):**
Protects against common web exploits (SQL injection, XSS, bot traffic). I already use Cloudflare's WAF, but AWS WAF would provide:
- AWS Managed Rules for OWASP Top 10 protection
- Rate limiting per IP or API key
- Geo-blocking (block traffic from specific countries)
- Custom rules based on request patterns

**AWS Shield:**
DDoS protection. Shield Standard is free and automatic; Shield Advanced ($3,000/month) provides enhanced protection and cost protection guarantees. For a student project, Shield Standard is sufficient, but clients might want Shield Advanced recommendations.

**AWS Network Firewall:**
Stateful network inspection at the VPC level. This is what I've been researching for my dashboard's network security enhancement. Network Firewall can:
- Inspect all traffic entering/leaving the VPC
- Block traffic to known malicious domains
- Enforce rules based on protocol, port, and packet inspection
- Integrate with threat intelligence feeds

The session confirmed my architectural plan: place Network Firewall in an inspection VPC and route all traffic through it for centralized security policy enforcement.

**Workload Protection - EC2/ECS/EKS:**
Key practices for compute security:
- **Minimize attack surface:** Remove unnecessary packages, close unused ports, disable unneeded services
- **Patch management:** Use AWS Systems Manager Patch Manager for automated OS patching
- **Container security:** Scan images for vulnerabilities (ECR image scanning), use minimal base images, run as non-root users
- **Secrets management:** Never bake secrets into AMIs or container images; use Secrets Manager or Parameter Store

For my ECS migration, I should:
- Use AWS-provided base images (regularly patched)
- Enable ECR image scanning to detect vulnerabilities before deployment
- Run containers as non-root user
- Mount secrets as environment variables from Secrets Manager at runtime

**Key Takeaway for My Project:**
Restructure VPC with proper subnet segmentation (public/private/isolated), migrate from public to private subnet placement during ECS migration, implement AWS WAF on ALB, configure Network Firewall in inspection VPC, and establish container security baseline (image scanning, non-root execution, secrets management).

---

## Pillar 4: Data Protection (10:40 - 11:10 AM)

Data protection is where encryption, key management, and access controls converge.

**KMS - Key Management Service:**
The deep dive into KMS revealed complexities I hadn't fully appreciated:

**Key Policies vs. IAM Policies:**
KMS keys have their own resource policies (key policies) separate from IAM policies. A principal needs both KMS key policy permissions AND IAM policy permissions to use a key. This dual-authorization model provides defense in depth.

For my DynamoDB encryption, I should:
- Create a customer-managed KMS key (instead of AWS-managed default)
- Configure key policy to allow only my application role to use the key
- Enable automatic key rotation (yearly)
- Use grants for temporary access (better than modifying key policy repeatedly)

**Encryption at Rest:**
Every storage service should encrypt at rest:
- **S3:** Default encryption with SSE-S3 or SSE-KMS (prefer KMS for audit logs)
- **EBS:** Encrypt all volumes with KMS
- **RDS/Aurora:** Enable encryption at cluster creation (can't add later without migration)
- **DynamoDB:** Already encrypted by default, but use customer-managed KMS for better control

My current implementation uses default encryption for DynamoDB, which is acceptable but limits visibility. Customer-managed keys would provide:
- CloudTrail logs showing every decrypt operation (who accessed data, when, from where)
- Ability to disable key in emergency (immediately denies all access to data)
- Cross-account key sharing for multi-tenant scenarios

**Encryption in Transit:**
The session emphasized TLS everywhere:
- Public-facing endpoints: TLS 1.2+ (I already have this via Cloudflare)
- Internal service communication: Even private subnet traffic should use TLS
- Database connections: Force SSL/TLS for RDS, use encrypted Redis connections

My current backend → Redis connection likely doesn't use TLS since both are on localhost. When migrating to ElastiCache, I must enable encryption in transit.

**Secrets Manager vs. Parameter Store:**
Both store secrets, but Secrets Manager provides:
- **Automatic rotation:** Built-in rotation for RDS, Redshift, DocumentDB credentials
- **Cross-region replication:** Disaster recovery for secrets
- **Fine-grained access control:** Secrets can have resource policies

Parameter Store is cheaper (free tier) but requires manual rotation logic. For production, Secrets Manager is worth the cost (~$0.40/secret/month + $0.05/10K API calls).

I should migrate my JWT secret, database credentials, and AWS API keys from environment variables or Systems Manager Parameter Store to Secrets Manager with automatic rotation.

**Rotation Patterns:**
The rotation demonstration showed a zero-downtime pattern:
1. Secrets Manager creates new credential version
2. Old and new credentials both valid simultaneously
3. Application gradually picks up new version
4. Old version deprecated after all connections migrated
5. Old version deleted

This is critical for my multi-tenant dashboard where clients might have long-running connections. Immediate rotation would break active sessions.

**Data Classification & Access Guardrails:**
The session introduced AWS Macie for automated data classification. Macie scans S3 buckets and identifies:
- Personally Identifiable Information (PII)
- Credit card numbers
- API keys and credentials
- Intellectual property

For clients storing sensitive data in S3, I should recommend Macie integration to automatically detect and alert on PII exposure.

**Key Takeaway for My Project:**
Migrate to customer-managed KMS keys for DynamoDB with audit logging, implement Secrets Manager with automatic rotation for all credentials, enable encryption in transit for all service communication including Redis/ElastiCache, add Macie scanning to dashboard for clients with S3 buckets, and document data classification requirements for multi-tenant data segregation.

---

## Pillar 5: Incident Response (11:10 - 11:40 AM)

The incident response session was the most immediately actionable content of the day.

**IR Lifecycle According to AWS:**
The framework consists of five phases:
1. **Prepare:** Build IR tools, runbooks, train team, establish communication channels
2. **Detect:** Use GuardDuty, Security Hub, CloudWatch Alarms to identify incidents
3. **Analyze:** Investigate scope, affected resources, attack timeline using CloudTrail and logs
4. **Contain:** Isolate affected resources, prevent further damage
5. **Recover:** Restore from clean backups, patch vulnerabilities, return to normal operations

**Playbook 1: Compromised IAM Key**

The demonstration walked through a realistic scenario where an access key was leaked to GitHub:

*Detection:* GuardDuty finding "UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration" or Access Analyzer alert

*Analysis:*
- CloudTrail logs show which actions were taken with compromised key
- Check origin IP addresses (is this expected location?)
- Determine scope: which resources were accessed/modified?

*Containment:*
```bash
# Immediately disable the compromised access key
aws iam update-access-key --access-key-id AKIA... --status Inactive --user-name compromised-user

# Attach explicit deny policy to prevent any actions
aws iam put-user-policy --user-name compromised-user --policy-name DenyAll --policy-document '{...}'
```

*Recovery:*
- Generate new access key for legitimate user
- Review all resources accessed during compromise period
- Check for backdoors (new IAM users, roles, lambda functions)
- Rotate any secrets that may have been exposed

This playbook is directly applicable to my dashboard. I should build an automated response:
- EventBridge rule triggered by GuardDuty finding
- Lambda function that disables key and applies deny policy
- SNS notification to security team
- Create CloudWatch dashboard showing compromised key activity timeline

**Playbook 2: S3 Public Exposure**

Scenario: S3 bucket accidentally made public

*Detection:* Security Hub finding "S3.1 - S3 Block Public Access setting should be enabled" or EventBridge event "S3:PutBucketAcl"


3. **Collect Evidence:**
- Memory dump (if needed for forensics)
- Copy relevant logs to secure S3 bucket
- Document timeline of events from CloudTrail

4. **Terminate or Remediate:**
- If infrastructure-as-code: Terminate and redeploy clean version
- If persistent: Scan with antivirus, remove malware, patch vulnerabilities

The key insight is **immutable infrastructure**: treating compromised instances as disposable and rebuilding from known-good images is faster and more reliable than trying to clean infected systems.

**Auto-Response with Lambda + Step Functions**

The session demonstrated a Step Functions workflow for sophisticated incident response:

```yaml
IR-Workflow:
  - Detect (EventBridge trigger)
  - Snapshot (forensic preservation)
  - Isolate (quarantine security group)
  - Analyze (Lambda inspection)
  - Decision:
      If severity HIGH:
        - Terminate instance
        - Notify security team
        - Create incident ticket
      If severity MEDIUM:
        - Keep quarantined
        - Request manual review
        - Schedule remediation
  - Recover (rebuild from clean AMI)
  - Document (update SIEM/case management)
```

This orchestration handles complex scenarios with human-in-the-loop decision points for medium-severity incidents while fully automating high-severity responses.

**Key Takeaway for My Project:**
Build incident response playbooks into dashboard as automated runbooks, implement EventBridge + Lambda auto-response for compromised keys and public S3 buckets, create snapshot-before-remediation pattern for forensic preservation, develop Step Functions workflows for complex IR scenarios, and add IR timeline visualization to dashboard showing attack progression from CloudTrail logs.

---
## Conclusion

The AWS Well-Architected Security Pillar workshop provided a comprehensive framework for production-grade cloud security. The five pillars—Identity & Access Management, Detection, Infrastructure Protection, Data Protection, and Incident Response—are interconnected layers that create defense in depth.

The most valuable insight was the shift from reactive security (responding to alerts) to proactive security (automated prevention and response). By codifying security controls in infrastructure-as-code, implementing automated incident response, and continuously monitoring across all pillars, organizations can maintain security at scale without proportionally increasing headcount.

For my Cloud Health Dashboard project, this workshop outlined a clear roadmap from basic monitoring to comprehensive security operations platform. The technical knowledge gained will directly improve my architecture while the certifications roadmap provides career progression guidance.

The networking opportunity with other Vietnamese engineers also revealed that security challenges are universal—every startup struggles with IAM complexity, multi-account architectures, and balancing security with development velocity. The solutions presented today are battle-tested patterns that apply across industries and company sizes.

---
