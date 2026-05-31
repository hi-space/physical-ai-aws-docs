---
description: Reference architecture for running batch inference using Cosmos containers in AWS Batch
---

# AWS Batch + EC2

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

AWS Batch is a **fully managed service** that runs batch computing workloads at scale. It is well-suited for deploying Cosmos WFM in offline inference scenarios.

This architecture lets you process large-scale Physical AI data **without running permanent infrastructure continuously**. It is particularly well-suited for tasks such as synthetic trajectory generation, scene transformation generation, and environmental prediction.

The deployment uses a **container-based Cosmos model** orchestrated by AWS Batch. It automatically provisions the optimal compute resources (GPU-equipped EC2 instances) based on job queue demand.

* Input data from [Amazon S3](https://aws.amazon.com/s3/) or [Amazon EFS](https://aws.amazon.com/efs/) triggers batch jobs. Jobs perform inference tasks such as video generation, scene completion, and physics simulation.
* Results are written to EFS and can be reused later in robot learning pipelines or simulation workflows.
* Monitoring can be configured via [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/). [AWS IAM](https://aws.amazon.com/iam/) policies enforce **least privilege** access to model artifacts and data repositories.



**Advantages**

* **Cost Optimization**: Dynamic scaling provisions GPU resources only when inference jobs are running. Instances are terminated when work completes, reducing idle infrastructure costs. This is especially beneficial for intermittent workloads (dataset augmentation, large-scale synthetic data generation). Using Spot instances can lower costs further.
* **Simplified Operations**: Provides automatic job scheduling, resource provisioning, dependency management, and retry logic. You can focus on model and pipeline optimization rather than cluster operations.
* **Large-Scale Data Generation Throughput**: Scales seamlessly from single jobs to thousands of parallel inference tasks. Rapidly processes large datasets for Physical AI training, accelerating the iteration speed of robot policy development and autonomous system validation.
