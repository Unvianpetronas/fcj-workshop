---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 302
chapter: false
pre: " <b> 3.2. </b> "
---

# Migrate and modernize VMware workloads with AWS Transform for VMware

*by Kiran Reid, Ramu Akula, Patrick Kremer, and Mark Jaggers on 08 JUL 2025 in [AWS for VMware](https://aws.amazon.com/blogs/architecture/category/compute/aws-for-vmware/), [AWS Transform](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/aws-transform/), [Generative AI](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/)*

On May 15, 2025, AWS unveiled a game-changing solution: [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/). This innovative service tackles head-on the longstanding challenges of cloud migration, ushering in a new era of streamlined, efficient transitions to the AWS Cloud. By significantly reducing manual effort and accelerating the migration of critical VMware workloads, AWS Transform for VMware is set to revolutionize how organizations approach their cloud journey.

Since its [general availability announcement](https://aws.amazon.com/blogs/migration-and-modernization/new-capabilities-in-aws-transform-for-vmware/), AWS Transform for VMware has ignited enthusiasm across industries, with organizations eager to leverage its capabilities to accelerate their VMware workload migration and modernization initiatives. As we dive into the intricacies of this transformative technology, we'll uncover how AWS Transform for VMware is not just simplifying migrations, but reshaping the very landscape of cloud adoption and digital transformation.

## The VMware migration challenge

Moving enterprise workloads to the cloud isn't just a technical challenge – it's a business transformation that demands precision, speed, and minimal disruption. Years of established operational processes have often led to complex environments with poorly documented configurations, inconsistent security practices, and heavy reliance on institutional knowledge. Technical teams must navigate intricate application dependencies, coordinate across multiple stakeholders, and maintain business continuity while executing these transformational projects. The lack of comprehensive documentation and clear understanding of system inter-dependencies frequently results in extended migration timelines and increased project risks. Additionally, the need to balance ongoing operations with migration activities presents challenges. Achieving proper knowledge transfer adds another layer of complexity to these critical initiatives.

## Solution overview

Let's explore how AWS Transform for VMware simplifies application discovery, automates network conversion, and orchestrates complex migrations through its comprehensive architecture in the following diagram.

![To understand how these capabilities work together, let's examine each component of the architecture.](/images/3-blog/2-image/Screenshot-2025-07-06-at-18.42.20.png)

## Streamlined discovery and assessment

The journey begins with a thorough discovery and assessment of your VMware environment (1). [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) (4) supports multiple discovery methods. One option is [RVTools](https://www.robware.net/) for VMware inventory collection. For customers running VMware NSX, there's optional [import/export](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/) functionality. Additionally, [AWS Application Discovery Service](https://aws.amazon.com/application-discovery/) offers both agent-based and agentless discovery options (2) to gather and collect data and dependencies for migration.

The Inventory Discovery capability (5) collects crucial data from your source environment and stores it securely in [Amazon Simple Storage Service](https://aws.amazon.com/s3/) (Amazon S3) buckets (12) within the AWS Migration Discovery Account (7). This data forms the foundation for informed migration planning and is further processed by AWS Application Discovery Service (15) in the AWS Migration Planning Account. AWS Transform works together with these services to provide a single place to track migration progress and collect server inventory and dependency data, which is essential for successful application grouping and wave planning.

## Intelligent network conversion and wave planning

With a comprehensive understanding of your environment, AWS Transform for VMware moves to the next critical phase. The Network Migration capability (19) automates the creation of [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates (13, 26) to set up the target network infrastructure. These templates ensure your cloud environment closely mirrors your source setup, simplifying the setup for the migration.

Meanwhile, the Wave Planning capability (6) uses advanced [graph neural networks](https://en.wikipedia.org/wiki/Graph_neural_network) to analyze application dependencies and plan optimal migration waves. This minimizes complex portfolio and application dependency analysis, and provides ready-to-migrate wave plans, resulting in smooth migrations.

## Enhanced security and compliance

Security remains paramount throughout the migration process. [AWS Key Management Service](https://aws.amazon.com/kms/) (AWS KMS) (8, 16, 26) provides robust encryption for stored data, conversation history, and artifacts. By default, AWS managed keys are used, with the option to use [customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) (CMKs) for additional control.

[AWS Organizations](https://aws.amazon.com/organizations/) (9) enables centralized management across multiple AWS accounts, and [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) (14, 26) captures and logs API activities for a complete audit trail. Access control is managed through [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) (26), providing centralized access management across AWS accounts.

[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) (10, 26) continuously monitors AWS Transform service activities, resource utilization, and operational metrics within the management account, providing full visibility and control throughout the migration process. [AWS Identity Center](https://aws.amazon.com/iam/identity-center/) (11) further enhances security by providing centralized access management across all AWS accounts involved in the migration.

## Orchestrated migration execution

When it's time to execute the migration, AWS Transform orchestrates the end-to-end migration by coordinating across various AWS tools and services (20). The [AWS Application Migration Service](https://aws.amazon.com/application-migration-service/) (25) replicates servers from your source environment to [Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (Amazon EC2) instances (21) in the AWS Migration Target Account (18), based on the carefully planned waves and groupings.

The AWS Replication Agent (2) works in tandem with AWS Application Migration Service to ensure efficient and reliable data transfer. [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) (Amazon EBS) (21) provides the necessary storage for the migrated virtual machines, ensuring optimal performance and scalability.

## Flexible network configuration

AWS Transform for VMware offers two networking models to suit different requirements:

- **Hub-and-spoke model** – [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) (23) connects virtual private clouds (VPCs) through a central hub VPC with shared [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html). This model is ideal for centralized management and shared services.
- **Isolated model** – Each VPC operates independently with no connectivity established. This approach is designed for customers with existing AWS network infrastructure, enabling you to manually connect the new VPCs to your existing network topology.

VPCs (22) created by AWS Transform match your on-premises network segments, providing a seamless transition. NAT gateways (24) provide outbound internet access for private subnets, maintaining security while enabling necessary connectivity. In hub-and-spoke architectures, centralized NAT gateways in the hub VPC can serve multiple spoke VPCs, optimizing costs and simplifying management. For isolated VPC deployments, dedicated NAT gateways must be provisioned within each VPC requiring internet access. In all cases, you must configure route tables to enable egress traffic flow through the NAT gateways.

For complete setup instructions and requirements, refer to the [AWS Transform User Guide](https://docs.aws.amazon.com/transform/latest/userguide/what-is-service.html).

## Additional considerations

AWS Transform for VMware discovery workspaces are available globally (3). For the most up-to-date information on supported Regions, refer to [AWS Services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) (17).

Throughout the migration process, Amazon S3 buckets (12, 26) in both the AWS Migration Discovery Account and AWS Migration Target Account store key migration artifacts. These include inventory data, dependency mappings, wave plans, and application groupings, as well as Infrastructure as Code templates (AWS CloudFormation and AWS Cloud Development Kit) and per-wave migration plans.

## Customer benefits

AWS Transform for VMware delivers significant advantages:

- **Reduced manual effort** – Minimizes human error and frees up valuable IT resources through automation
- **Enhanced accuracy** – Uses AI-driven dependency mapping and wave planning for optimal migration strategies
- **Improved collaboration** – Centralized management and tracking foster better cross-team coordination
- **Cost optimization** – Right-sizes instances and takes advantage of AWS's flexible pricing models for immediate and long-term savings
- **Future-proofing** – Opens up opportunities for ongoing modernization and innovation on the AWS Cloud platform

Always review and follow your organization's security requirements, compliance obligations, and [AWS security best practices](https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all) when implementing any migration solution. For detailed security guidance, consult the [AWS Security Documentation](https://docs.aws.amazon.com/security/) and your organization's security team.

## Pricing

AWS Transform accelerates migration and modernization projects for VMware workloads with agentic AI capabilities. Currently, we offer our core features—including assessment and transformation—at no cost* to AWS customers. This allows you to speed up your migration and modernization journey without upfront expenses.

***No cost refers to the AWS Transform service itself. Standard charges apply for AWS services and resources used during migrations.**

## Summary and next steps

AWS Transform for VMware empowers organizations to overcome the complexities of VMware migration and modernization. By providing a comprehensive, automated approach, it enables faster, more reliable transitions to the AWS Cloud. This new service offers the tools and capabilities needed to navigate the changing VMware landscape confidently.

The architecture we explored demonstrates how AWS Transform for VMware tackles key challenges:

- Streamlines discovery and assessment processes
- Automates network conversion and intelligent wave planning
- Orchestrates migration execution with minimal disruption
- Enhances security and compliance throughout the migration
- Provides centralized management and monitoring
- Offers flexible networking options to suit diverse requirements

Ready to accelerate your VMware migration journey? Visit the [AWS Transform for VMware](https://aws.amazon.com/transform/vmware/) product page to learn more and get started today. Check out the following [interactive demo](https://aws.storylane.io/share/qye0se68an9i?utm_source=pdp) of AWS Transform for VMware. If you're exporting your network configuration from a VMware NSX environment, also refer to [Exporting network configuration data with Import/Export for NSX](https://aws.amazon.com/blogs/migration-and-modernization/exporting-network-configuration-data-with-import-export-for-nsx/). Our team of experts is ready to guide you through your migration and modernization initiatives, helping you unlock the full potential of the AWS Cloud.

---

## About the authors

![Kiran Reid](/images/3-blog/2-image/Screenshot-2025-05-28-at-09.14.50.png)
### Kiran Reid

Kiran Reid is a Senior Generative AI and Machine Learning Specialist at Amazon Web Services (AWS). With expertise in artificial intelligence (AI) technologies, Kiran works closely with AWS partners and customers to facilitate the migration and modernization of workloads using AI.

![Ramu Akula](/images/3-blog/2-image/akulrama.jpeg)
### Ramu Akula

Ram Akula is a Principal Portfolio Manager and Telco Network Transformation specialist in Amazon Web Services (AWS). With over 24 years of IT experience, he helps customers in migrating and modernizing their workloads to AWS.

![Patrick Kremer](/images/3-blog/2-image/Patrick-Kremer.png)
### Patrick Kremer

Patrick Kremer is a Senior Specialist Solutions Architect focused on Infrastructure Migration and Modernization. Patrick joined AWS in 2022, where he leverages his 20 years of VMware experience helping customers move to AWS solutions. He is a certified AWS Solutions Architect Professional with a passion for teaching and blogging.

![Mark Jaggers](/images/3-blog/2-image/markjagg.jpeg)
### Mark Jaggers

Mark Jaggers is a Manager of Outbound Product Management at Amazon Web Services (AWS), where he leads go-to-market strategies for cloud migration and disaster recovery solutions. Mark works within the service teams for both AWS Elastic Disaster Recovery and the AWS Application Migration Service, helping customers modernize their IT infrastructure at scale.
