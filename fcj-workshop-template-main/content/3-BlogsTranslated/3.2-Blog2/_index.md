---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 302
chapter: false
pre: " <b> 3.2. </b> "
---

# Migrate and modernize VMware workloads with AWS Transform for VMware

*by Kiran Reid, Ramu Akula, Patrick Kremer, and Mark Jaggers on 08 JUL 2025 in [AWS for VMware](https://aws.amazon.com/blogs/architecture/category/compute/aws-for-vmware/), [AWS Transform](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/aws-transform/), [Generative AI](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/)*

On May 15, 2025, AWS announced a groundbreaking solution: [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/). This innovative service directly addresses the longstanding challenges of cloud migration, ushering in a new era of streamlined and efficient transitions to the AWS Cloud. By significantly reducing manual effort and accelerating the migration of critical VMware workloads, AWS Transform for VMware is set to revolutionize how organizations approach their cloud journeys.

Since its [general availability launch](https://aws.amazon.com/blogs/migration-and-modernization/new-capabilities-in-aws-transform-for-vmware/), AWS Transform for VMware has sparked excitement across industries, with organizations eager to harness its capabilities to accelerate their VMware workload migration and modernization projects. As we delve into the intricacies of this transformative technology, we'll explore how AWS Transform for VMware not only simplifies migration but also reshapes the landscape of cloud adoption and digital transformation.

## The challenges of VMware migration

Migrating enterprise workloads to the cloud is not merely a technical challenge—it's a business transformation requiring precision, speed, and minimal disruption. Years of established operational processes often result in complex environments with poorly documented configurations, inconsistent security practices, and heavy reliance on institutional knowledge. Technical teams must navigate intricate application dependencies, coordinate with multiple stakeholders, and maintain business continuity while executing these transformative projects. The lack of comprehensive documentation and clear understanding of system interdependencies often results in prolonged migration timelines and increased project risk. Additionally, the need to balance ongoing operations with migration activities creates resource constraints. Proper knowledge transfer adds another layer of complexity to these critical initiatives.

## Solution overview

Let's explore how AWS Transform for VMware simplifies application discovery, automates network transformation, and orchestrates complex migrations through its comprehensive architecture in the following diagram.

![To understand how these capabilities work together, consider each component of the architecture.](/images/3-blog/2-image/Screenshot-2025-07-06-at-18.42.20.png)

## Streamlined discovery and assessment

The journey begins with a thorough discovery and assessment of your VMware environment (1). [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) (4) supports multiple discovery methods. One option is [RVTools](https://www.robware.net/) for gathering VMware inventory information. For customers running VMware NSX, there's an optional [import/export](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/) capability. Alternatively, [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/) provides both agentless and agent-based discovery options (2) to collect data and dependencies for migration.

The Inventory Discovery capability (5) gathers critical data from your source environment and securely stores it in [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) buckets (12) within the AWS Migration Discovery Account (7). This data forms the foundation for informed migration planning and is further processed by AWS Application Discovery Service (15) in the AWS Migration Planning Account. AWS Transform works in conjunction with these services to provide a single place to track migration progress and collect server inventory data and dependencies, which are essential for successful application grouping and wave planning.

## Intelligent network transformation and wave planning

With a comprehensive understanding of your environment, AWS Transform for VMware moves to the next critical phase. The Network Migration capability (19) automates the creation of [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates (13, 26) to establish target network infrastructure. These templates ensure your cloud environment closely mirrors your source setup, simplifying the setup for migration.

Meanwhile, the Wave Planning capability (6) uses advanced [graph neural networks](https://en.wikipedia.org/wiki/Graph_neural_network) to analyze application dependencies and plan optimal migration waves. This reduces complex portfolio and application dependency analysis, and provides migration-ready wave plans, resulting in smoother migrations.

## Enhanced security and compliance

Security remains paramount throughout the migration process. [AWS Key Management Service](https://aws.amazon.com/kms/) (AWS KMS) (8, 16, 26) provides robust encryption for stored data, chat history, and artifacts. By default, AWS-managed keys are used, with the option to use [customer-managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) (CMKs) for additional control.

[AWS Organizations](https://aws.amazon.com/organizations/) (9) enables centralized management across multiple AWS accounts, and [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) (14, 26) records and stores logs of API activities for a complete audit trail. Access controls are managed through [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) (26), providing centralized access management across AWS accounts.

[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) (10, 26) continuously monitors AWS Transform service activities, resource usage, and operational metrics in the management account, providing full visibility and control throughout the migration. [AWS Identity Center](https://aws.amazon.com/iam/identity-center/) (11) further enhances security by providing centralized access management across all AWS accounts involved in the migration.

## Orchestrated migration execution

When it's time to execute the migration, AWS Transform orchestrates end-to-end migration by coordinating across various AWS tools and services (20). [AWS Application Migration Service](https://aws.amazon.com/application-migration-service/) (25) replicates servers from your source environment to [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2) instances (21) in the AWS Migration Target Account (18), based on carefully planned waves and groups.

AWS Replication Agent (2) works in parallel with AWS Application Migration Service to ensure efficient and reliable data transfer. [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) (Amazon EBS) (21) provides the necessary storage for migrated virtual machines, ensuring optimal performance and scalability.

## Flexible network configuration

AWS Transform for VMware offers two network models to suit different requirements:

- **Hub and spoke model** – [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) (23) connects virtual private clouds (VPCs) through a central VPC with shared [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html). This model is ideal for centralized management and shared services.
- **Standalone model** – Each VPC operates independently without any connectivity established. This approach is designed for customers with existing AWS network infrastructure, allowing you to manually connect new VPCs to your existing network fabric.

VPCs (22) created by AWS Transform match your on-premises network segments, providing a seamless transition. NAT gateways (24) provide outbound internet access for private subnets, maintaining security while enabling necessary connectivity. In the hub and spoke architecture, centralized NAT gateways in the hub VPC can serve multiple spoke VPCs, optimizing costs and simplifying management. For standalone VPC deployments, dedicated NAT gateways must be provisioned within each VPC requiring internet access. In all cases, you must configure route tables to enable outbound traffic flow through NAT gateways.

For complete setup guidance and requirements, refer to the [AWS Transform User Guide](https://docs.aws.amazon.com/transform/latest/userguide/what-is-service.html).

## Additional considerations

AWS Transform for VMware discovery workspaces are available globally (3). For the most current information on supported Regions, refer to [AWS services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) (17).

Throughout the migration process, Amazon S3 buckets (12, 26) in both the AWS Migration Discovery Account and the AWS Migration Target Account store key migration artifacts. These include inventory data, dependency mappings, wave plans and application groups, as well as Infrastructure as Code templates (AWS CloudFormation and AWS Cloud Development Kit) and wave-by-wave migration plans.

## Customer benefits

AWS Transform for VMware delivers significant advantages:

- **Reduced manual effort** – Minimize human error and free up valuable IT resources through automation
- **Increased accuracy** – Use AI-driven dependency mapping and wave planning for optimal migration strategies
- **Improved collaboration** – Centralized management and tracking promote better coordination across teams
- **Cost optimization** – Right-size instances and take advantage of AWS's flexible pricing models for immediate and long-term savings
- **Future-proofing** – Unlock opportunities for continuous modernization and innovation on the AWS Cloud platform

Always review and adhere to your organization's security requirements, compliance obligations, and [AWS security best practices](https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all) when implementing any migration solution. For detailed security guidance, refer to [AWS Security Documentation](https://docs.aws.amazon.com/security/) and your organization's security team.

## Pricing

AWS Transform accelerates migration and modernization projects for VMware workloads with agentic AI capabilities. Currently, we offer our core features—including assessment and transformation—at no cost* to AWS customers. This enables you to accelerate your migration and modernization journey without upfront costs.

***At no cost refers to the AWS Transform service itself. Standard charges apply for AWS services and resources used during migration.**

## Summary and next steps

AWS Transform for VMware empowers organizations to navigate the complexities of VMware migration and modernization. By providing a comprehensive, automated approach, it enables faster, more reliable transitions to the AWS Cloud. This new service provides the tools and capabilities necessary to navigate the changing VMware landscape with confidence.

The architecture we've explored demonstrates how AWS Transform for VMware addresses key challenges:

- Streamlines discovery and assessment processes
- Automates network transformation and intelligent wave planning
- Orchestrates migration execution with minimal disruption
- Enhances security and compliance throughout the migration
- Provides centralized management and monitoring
- Offers flexible network options to fit diverse requirements

Ready to accelerate your VMware migration journey? Visit the [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) product page to learn more and get started today. Check out the following [interactive demo](https://aws.storylane.io/share/qye0se68an9i?utm_source=pdp) of AWS Transform for VMware. If you're exporting network configuration from your VMware NSX environment, also refer to [Exporting network configuration data with Import/Export for NSX](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/). Our team of experts is ready to guide you through your migration and modernization initiatives, helping you unlock the full potential of the AWS Cloud.

---

## About the authors

![Kiran Reid](/images/3-blog/2-image/Screenshot-2025-05-28-at-09.14.50.png)
### Kiran Reid

Kiran Reid is a Senior Generative AI and Machine Learning Specialist at Amazon Web Services (AWS). With expertise in artificial intelligence (AI) technology, Kiran works closely with AWS partners and customers to support migration and modernization of workloads with AI.

![Ramu Akula](/images/3-blog/2-image/akulrama.jpeg)
### Ramu Akula

Ram Akula is a Principal Portfolio Manager and Telecom Transformation Specialist at Amazon Web Services (AWS). With over 24 years of IT experience, he helps customers migrate and modernize their workloads to AWS.

![Patrick Kremer](/images/3-blog/2-image/Patrick-Kremer.png)
### Patrick Kremer

Patrick Kremer is a Senior Specialist Solutions Architect focusing on Infrastructure Migration and Modernization. Patrick joined AWS in 2022, where he leverages his 20 years of VMware experience to help customers transition to AWS solutions. He is a Certified AWS Solutions Architect Professional with a passion for teaching and blogging.

![Mark Jaggers](/images/3-blog/2-image/markjagg.jpeg)
### Mark Jaggers

Mark Jaggers is an Outbound Product Management Manager at Amazon Web Services (AWS), where he leads go-to-market strategies for cloud migration and disaster recovery solutions. Mark works in the service teams for both AWS Elastic Disaster Recovery and AWS Application Migration Service, helping customers modernize their IT infrastructure at scale.