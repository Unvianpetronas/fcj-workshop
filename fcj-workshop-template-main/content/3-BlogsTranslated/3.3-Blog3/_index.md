---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 303
chapter: false
pre: " <b> 3.3. </b> "
---

# Deploy LLMs on Amazon EKS using vLLM Deep Learning Containers

*by Vishal Naik and Sumeet Tripathi on 14 AUG 2025 in [AWS Deep Learning AMIs](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/aws-deep-learning-amis/), [Best Practices](https://aws.amazon.com/blogs/architecture/category/post-types/best-practices/), [Expert (400)](https://aws.amazon.com/blogs/architecture/category/learning-levels/expert-400/), [Generative AI](https://aws.amazon.com/blogs/architecture/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/architecture/category/post-types/technical-how-to/)*

Organizations face significant challenges when deploying large language models (LLMs) efficiently at scale. Key challenges include optimizing GPU resource utilization, managing network infrastructure, and providing efficient access to model weights. When running distributed inference workloads, organizations often encounter complexities in orchestrating model operations across multiple nodes. Common challenges include efficiently distributing model components across available GPUs, coordinating seamless communication between processing units, and maintaining consistent performance with low latency and high throughput.

[vLLM](https://docs.vllm.ai/en/latest/) is an open-source library for fast LLM inference and serving. [vLLM AWS Deep Learning Containers (DLCs)](https://docs.aws.amazon.com/deep-learning-containers/latest/devguide/dlc-vllm-0-8-x86-ec2.html) are optimized for customers deploying vLLMs on [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2) (Amazon EC2), [Amazon Elastic Container Service](http://aws.amazon.com/ecs) (Amazon ECS), and [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), and are provided at no cost. These containers package a preconfigured environment, tested to work seamlessly from the start, including necessary dependencies like drivers and libraries to run vLLMs efficiently, and provide integrated support for [Elastic Fabric Adapter](https://aws.amazon.com/hpc/efa/) (EFA) for high-performance multi-node inference workloads. You no longer need to build an inference environment from scratch. Instead, you can install vLLM DLC and it will automatically set up and configure the environment, and you can start deploying inference workloads at scale.

In this post, we demonstrate how to deploy the DeepSeek-R1-Distill-Qwen-32B model using AWS DLCs for vLLMs on Amazon EKS, showcasing how these purpose-built containers simplify the deployment of this powerful open-source inference engine. This solution can help you address the complex infrastructure challenges of deploying LLMs while maintaining performance and cost efficiency.

## AWS DLCs

AWS DLCs provide generative AI practitioners with optimized Docker environments for training and deploying generative AI models in their pipelines and workflows on Amazon EC2, Amazon EKS, and Amazon ECS. AWS DLCs are targeted at self-managed machine learning (ML) customers who want to build and maintain their own AI/ML environment, want control at the infrastructure instance level, and manage their own training and inference workloads. DLCs are available as Docker images for training and inference, and also with PyTorch and TensorFlow. DLCs are updated with the latest versions of frameworks and drivers, tested for compatibility and security, and provided at no cost. They can also be quickly customized by following our cookbook instructions. Using AWS DLCs as a building block for generative AI environments reduces the burden on operations and infrastructure teams, reduces TCO for AI/ML infrastructure, accelerates generative AI product development, and helps generative AI teams focus on value-added work to gain insights provided by generative AI from organizational data.

## Solution overview

The following diagram shows the interaction between Amazon EKS, GPU-enabled EC2 instances with EFA networking, and [Amazon FSx for Lustre](https://aws.amazon.com/fsx/lustre/) storage. Requests from clients flow through an Application Load Balancer (ALB) to vLLM server pods running on EKS nodes, accessing model weights stored on FSx for Lustre. This architecture provides a scalable, high-performance solution for serving LLM inference workloads with optimal cost efficiency.

![Solution architecture](/images/3-blog/3-image/image-1-4.png)

The following diagram illustrates the DLC stack on AWS. This stack presents a comprehensive architecture from the EC2 instance foundation through the container runtime, essential GPU drivers, and ML frameworks like PyTorch. The layered diagram shows how CUDA, NCCL, and other critical components integrate to support high-performance deep learning workloads.

![DLC stack](/images/3-blog/3-image/image-2-3.png)

vLLM DLCs are specifically optimized for high-performance inference, with integrated support for tensor parallelism and pipeline parallelism across multiple GPUs and nodes. This optimization enables efficient scaling of large models like DeepSeek-R1-Distill-Qwen-32B, which would be challenging to deploy and manage. The containers also include optimized CUDA configuration and EFA drivers, facilitating maximum throughput for distributed inference workloads. This solution uses the following AWS services and components:

* **AWS DLCs for vLLMs** – Preconfigured Docker images optimized to simplify deployment and maximize performance
* **EKS cluster** – Provides Kubernetes control plane for container orchestration
* **P4d.24xlarge instances** – [EC2 P4d instances](https://aws.amazon.com/ec2/instance-types/p4/) with 8 NVIDIA A100 GPUs per instance, configured in a managed node group
* **Elastic Fabric Adapter** – Network interface enabling high-performance computing applications to scale efficiently
* **FSx for Lustre** – High-performance file system for storing model weights
* **LeaderWorkerSet pattern** – Custom Kubernetes resource for deploying vLLM in distributed configuration
* **AWS Load Balancer Controller** – Manages ALB for external access

By combining these components, we create an inference system that provides low-latency, high-throughput LLM serving with minimal operational overhead.

## Prerequisites

Before getting started, ensure you have the following prerequisites:

* An [AWS account](https://portal.aws.amazon.com/billing/signup#/start/email) with access to EC2 P4 instances (you may need to [request a quota increase](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html))
* Access to a terminal with the following tools installed:
  * [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) version 2.11.0 or newer
  * [eksctl](https://eksctl.io/installation/) version 0.150.0 or newer
  * [kubectl](https://kubernetes.io/docs/tasks/tools/) version 1.27 or newer
  * [Helm](https://helm.sh/docs/intro/install/) version 3.12.0 or newer
* An [AWS CLI profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) (vllm-profile) configured with [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) role or user having the following permissions:
  * Create, manage, and delete EKS clusters and node groups
  * Create, manage, and delete EC2 resources, including virtual private clouds (VPCs), subnets, security groups, and internet gateways
  * Create and manage IAM roles
  * Create, update, and delete [AWS CloudFormation](http://aws.amazon.com/cloudformation) stacks
  * Create, delete, and describe FSx file systems
  * Create and manage Elastic Load Balancers

This solution can be deployed in AWS Regions where Amazon EKS, P4d instances, and FSx for Lustre are available. This guide uses the us-west-2 Region. The complete deployment takes approximately 60–90 minutes.

Clone our GitHub repository containing the necessary configuration files:
```bash