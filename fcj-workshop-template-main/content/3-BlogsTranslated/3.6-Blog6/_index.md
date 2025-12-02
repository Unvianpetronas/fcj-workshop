---
title: "Blog 6"
date: "`r Sys.Date()`"
weight: 306
chapter: false
pre: " <b> 3.6. </b> "
---

# Use generative AI on AWS to efficiently analyze clinical documents

**Authors:** Alex Boudreau and John O'Donnell  
**Date:** February 05, 2025  
**Tags:** Amazon Bedrock, Amazon Comprehend Medical, Amazon Machine Learning, Amazon SageMaker, Best Practices, Customer Solutions, Generative AI

---

Clinical trials involve collecting and processing large volumes of highly regulated data, including complex protocol documents that describe how the trial will be conducted. Managing this volume of information can be overwhelming, but generative AI offers a solution by helping automate the process and enabling clinical researchers to quickly focus on the most relevant information. Currently, the average drug approval process takes 10–12 years, with clinical trial study startup time accounting for 1 year within that timeframe. Much of the challenge with study startup lies in the complex and non-standard nature of protocol documents. These documents often require many weeks or months to review and assess. This review time adds to the already lengthy cycle of bringing a new drug to market.

In this post, we show how Clario uses the AWS platform to accelerate clinical document analysis.

## About Clario

Clario is a leading provider of endpoint data solutions for the clinical trials industry delivering regulatory-grade clinical evidence for pharmaceutical, biotech, and medical device partners. Since Clario was founded over 50 years ago, their endpoint data solutions have supported clinical trials over 26,000 times with over 700 regulatory approvals across over 100 countries. One of the critical challenges that Clario faces is the time-consuming process of creating documentation for clinical trials, which can take many weeks or months.

## Business challenge

Clinical trials are essential for approving new health innovations, including treatments, procedures, and medical devices. They require collecting large volumes of complex data from dispersed clinical trial sites to support assessment of medical benefit and risk, all while maintaining privacy and regulatory compliance. To make the problem even more challenging, capturing data in clinical trials occurs not only in healthcare centers but also through remote capture across many aspects of the trial participant's daily activity.

Partners like Clario understand the challenges that life sciences companies face when analyzing large volumes of complex clinical documents, such as study protocols. These documents often contain a mixture of structured and unstructured data, including tables, images, and diagrams, making it difficult to accurately interpret and extract key information at scale. In this post, we explore how Clario has harnessed the power of generative AI on AWS to efficiently analyze clinical documents and drive better outcomes for their clients.

## Harnessing the power of large language models

The rapid advancement in large language models (LLMs) has expanded the potential applications of natural language processing beyond simple conversational AI assistants. Clario has experimented with many different techniques, such as zero-shot learning, few-shot learning, classification, entity extraction, and summarization, to effectively use LLMs in specialized use cases. By using prompt engineering, AI orchestration, and content retrieval, Clario can guide models to accurately generate insights and extract relevant information from key clinical research documents, including complex clinical trial protocols.

## Four pillars of effective document analysis on AWS

Through their research and development efforts, Clario has identified four core pillars that enable effective document analysis using generative AI on AWS:

**Parsing** – Clario uses AWS services like Amazon Textract and Amazon Comprehend to extract text, images, and tables from clinical documents, maintaining both data privacy and security.

**Retrieval** – By using embedding models and vector databases like Amazon OpenSearch Service, Clario efficiently stores and retrieves relevant information from large document collections based on similarity search. The team has experimented with many different chunking and retrieval strategies to optimize accuracy and performance.

**Prompting** – Using techniques like zero-shot and few-shot learning, Clario has enhanced LLM accuracy to classify and extract information. AWS services like Amazon Bedrock simplify experimenting with different prompting strategies and evaluating model performance.

**Generation** – Clario carefully considers factors like context size, reasoning capabilities, and latency when choosing appropriate LLMs to generate structured output. AWS provides a range of pre-trained models and frameworks that integrate seamlessly into Clario's pipeline.

## Solution overview

To address the unique challenges associated with analyzing clinical documents, Clario has built a custom generative AI platform on AWS. This platform combines an orchestration engine that incorporates multiple LLMs and deep learning models, enabling it to extract key information accurately and at scale. By using AWS services like Amazon Elastic Compute Cloud (Amazon EC2), Amazon Elastic Kubernetes Service (Amazon EKS), Amazon Simple Storage Service (Amazon S3), SageMaker, and AWS Lambda, Clario can efficiently process thousands of documents in seconds.

The following diagram illustrates the solution architecture.

![ARCHBLOG-1078-clario3-imaging-and-alexb-alex-edit-v3-Page-3.drawio-Copy.png](/images/3-blog/6-image/ARCHBLOG-1078-clario3-imaging-and-alexb-alex-edit-v3-Page-3.drawio-Copy.png)

The workflow includes the following steps:

1. Documents are collected on premises (1) and uploaded using AWS Direct Connect (2) with encryption in transit to Amazon S3 (3). All uploaded documents are then automatically and securely stored with server-side object-level encryption.

2. After documents are uploaded and users have reviewed them, the Clario AI Orchestration Engine (4) determines the best document parsing strategy based on file type and extracts text using Amazon Textract (5). After extraction, text is vectorized and stored in the Amazon OpenSearch Service vector engine (6) for later semantic retrieval.

3. After vectorization, the Clario AI Orchestration Engine (4), running as a distributed service in Amazon EKS, launches a document classification async task using Amazon MQ. Amazon EC2 and Lambda are used for additional processing if needed. This triggers the Document Classification Agent, using an Amazon Bedrock LLM (8), to automatically determine document type.

4. After documents are classified, the Clario AI Orchestration Engine (4) launches the appropriate document analysis agent for further background processing. In the case of study protocols, the engine launches the Protocol Analysis agent, using predefined analysis graph configurations stored in Amazon Relational Database Service (Amazon RDS) (7), as well as a combination of retrieval strategies and AI models, including custom deep learning models on SageMaker (9) and pre-trained LLMs on Amazon Bedrock (8). This orchestration provides advanced document analysis, transforming massive amounts of unstructured multi-modal data into structured data and insights.

5. After analysis, all structured data is then persisted into Amazon RDS (7) for later visualization, review, and querying.

## Recommendations and best practices

Based on their experience in developing and deploying generative AI solutions on AWS, Clario has learned the following best practices:

- Adopt an incremental and iterative development approach to gradually build and refine your models
- Follow standard machine learning approaches to evaluate and validate model performance using representative test sets
- Optimize the four pillars of document analysis before investing in fine-tuning and continuous pre-training of LLMs
- Tailor your approach for specific use cases, because not all problems require the same models or techniques

## Conclusion

By harnessing the power of generative AI on AWS, Clario has been able to efficiently analyze complex clinical trial documents and extract valuable insights for their clients in the life sciences industry. Through a combination of careful model selection, iterative development, and adherence to best practices, Clario has built a scalable and accurate document analysis pipeline using AWS. Unlock the full potential of your clinical trial data by applying these best practices with AWS generative AI solutions today.

## About the authors

![1624904528953.jpg](/images/3-blog/6-image/1624904528953.jpg)### Alex Boudreau

Alex Boudreau is Director of AI at Clario. He leads the company's innovative Generative AI department and oversees the development of the company's advanced multi-modal GenAI Platform, encompassing cutting-edge cloud engineering, AI engineering, and foundational AI research. With a distinguished career in AI and machine learning, Alex has previously pioneered Deep Learning speech analysis systems for automotive applications, led cloud-based enterprise fraud detection solutions, advanced conversational AI technology, and groundbreaking projects in medical image analysis. His expertise in leading high-impact initiatives positions him uniquely to push the boundaries of AI technology in the business world.

![1675893031202-1-1.jpg](/images/3-blog/6-image/1675893031202-1-1.jpg)### John O'Donnell

John O'Donnell is a Principal Solutions Architect at Amazon Web Services (AWS) where he provides CIO-level engagement and design for complex cloud-based solutions in the healthcare and life sciences (HCLS) industry. With over 20 years of hands-on experience, he has a proven track record of delivering value and innovation for HCLS customers globally. As a trusted technical leader, he has partnered with AWS teams to dive deep into customer challenges, propose outcomes, and ensure high-value, predictable, and successful cloud transformations. John is passionate about helping HCLS customers achieve their goals and accelerate their cloud native modernization efforts.

---