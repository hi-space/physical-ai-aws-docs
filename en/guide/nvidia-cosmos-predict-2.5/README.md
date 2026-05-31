# NVIDIA Cosmos Predict 2.5 Deployment

### 1. Overview

NVIDIA Cosmos Predict is a World Foundation Model (WFM) for Physical AI that predicts the future state of the world in video format from text, image, and video inputs.&#x20;

This guide provides two reference architectures and sample code for deploying [Cosmos Predict 2.5](https://research.nvidia.com/labs/dir/cosmos-predict2.5/) on AWS.

> For detailed information about Cosmos Predict, see the [Cosmos-Predict2.5](../../paper-review-tbd/world-foundation-model/cosmos-predict2.5.md "mention") paper review.

***

### 2. Model Specification & GPU Requirements

On AWS, you can typically deploy in two ways based on the following selection criteria:

* Latency requirements
* Inference traffic pattern (continuous/peak/intermittent)
* Cost constraints
* Integration points within the Physical AI pipeline (data/post-processing/training)

<table data-header-hidden><thead><tr><th width="184">-</th><th>NIM on EKS</th><th>AWS Batch</th></tr></thead><tbody><tr><td><strong>Use Case</strong></td><td>Real-time inference (API serving)</td><td>Large-scale offline batch processing</td></tr><tr><td><strong>Architecture</strong></td><td>NVIDIA NIM + EKS</td><td>AWS Batch + EC2</td></tr><tr><td><strong>Advantages</strong></td><td>Low latency, always available, auto-scaling</td><td><ul><li>Cost optimization (eliminate idle time, save 60-90% with Spot instances)</li><li>Elastic throughput</li></ul></td></tr><tr><td><strong>Best For</strong></td><td>Continuous real-time services, production environments</td><td>Non-real-time large-scale data generation, intermittent workloads</td></tr></tbody></table>

#### [**Option 1: Real-Time Inference**](cosmos-nim-+-eks.md)

Operate [NVIDIA NIM microservices](https://developer.nvidia.com/nim?sortBy=developer_learning_library%2Fsort%2Ffeatured_in.nim%3Adesc%2Ctitle%3Aasc) on [Amazon EKS](https://aws.amazon.com/eks/). The EKS + NIM pattern keeps GPU pods running continuously, prioritizing response latency and availability with low inference latency. It is suitable for interactive calls.

#### [Option 2: **Batch Inference**](aws-batch-+-ec2.md)

Run container-based models as jobs in [AWS Batch](https://aws.amazon.com/batch/). The AWS Batch pattern provisions compute only when needed, making it suitable for offline workloads. It prioritizes cost efficiency and elastic throughput.

***

### References

* [**\[AWS Blog\]** Running NVIDIA Cosmos world foundation models on AWS](https://aws.amazon.com/blogs/hpc/running-nvidia-cosmos-world-foundation-models-on-aws/)
* [**\[NVIDIA Research\]** Cosmos-Predict2.5](https://research.nvidia.com/labs/dir/cosmos-predict2.5/)
* [**\[Github\]** nvidia-cosmos/cosmos-predict2.5](https://github.com/nvidia-cosmos/cosmos-predict2.5)
* [**\[Github\]** NVIDIA/nim-deploy](https://github.com/NVIDIA/nim-deploy)
* [**\[HuggingFace\]** nvidia/Cosmos-Predict2.5-2B](https://huggingface.co/nvidia/Cosmos-Predict2.5-2B)
* [**\[HuggingFace\]** nvidia/Cosmos-Predict2.5-14B](https://huggingface.co/nvidia/Cosmos-Predict2.5-14B)
