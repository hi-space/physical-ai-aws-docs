---
description: Deploy Cosmos Reason-1-7B model using SageMaker JumpStart
---

# NVIDIA Cosmos Reason-1-7B Deployment

### 1. Cosmos-Reason1

Cosmos Reason is an open-source **Reasoning Vision Language Model (VLM)** designed for Physical AI and robotics. This model goes beyond simply recognizing images and understands **Space**, **Time**, and **Physics**.

Like humans, it leverages prior knowledge and common sense to make judgments about situations. Embodied agents can use it to plan the actions they should take next.

#### Architecture and Principles

Cosmos Reason understands complex physical dynamics through sophisticated **chain-of-thought** reasoning capabilities.

1. **Input Processing**: When video or images are input, they are transformed into tokens through a **vision encoder** and a specialized transformer called a **projector**.
2. **Integrated Analysis**: The transformed video tokens are combined with text prompts and passed to the core model.
3. **Deep Reasoning**: The core model, which combines an LLM module with various techniques, thinks step-by-step and generates logical, detailed responses.

In particular, this model has been post-trained on physical common sense and embodied reasoning data, undergoing supervised fine-tuning (SFT) and reinforcement learning (RL) to understand world dynamics without human labeling.

#### Key Use Cases

Cosmos Reason excels at handling diverse "long tail" scenarios in the physical world and brings innovation to the following fields:

**Robot Planning and Reasoning**

Serves as the brain for Vision Language Action (VLA) models in robots. When humanoids or autonomous driving robots receive complex commands in unfamiliar environments, they can use common sense to break down tasks and formulate execution plans.

**Video Analytics AI Agents**

Extract valuable insights from vast video data and perform root-cause analysis. Analyze recorded videos or live streams from city surveillance or industrial sites to logically understand the causes and flow of incidents.

**Data Curation and Annotation**

When developers build large, diverse training datasets, automate high-quality curation and annotation work to maximize development efficiency.

***

### 2. What is SageMaker JumpStart?

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

[Amazon SageMaker JumpStart](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) is a comprehensive model hub that accelerates AI model adoption and development. Without having to build complex infrastructure or perform model tuning from scratch, you can deploy hundreds of popular open-source and proprietary pre-trained models to your AWS account with just a few clicks.

* **Pre-trained Model Library**: Hundreds of popular open-source and proprietary pre-trained models
* **Flexible Fine-tuning and Training**: Some models offer options for fine-tuning to your data
* **Integrated Management Environment**: Easily perform the entire process from model deployment, fine-tuning, to performance evaluation in one place

#### Why Deploy the Cosmos Reason Model with SageMaker JumpStart?

**One-Click Deployment**

No need to manually configure complex containers or GPU optimization. Deploy the latest AI models with just a few clicks through the SageMaker console or Python SDK (or a few lines of code).

**Integrated with NVIDIA NIM for Optimized Performance**

[NVIDIA NIM](https://www.nvidia.com/ai-data-science/products/nim-microservices/) is a microservice that optimizes inference for NVIDIA accelerated infrastructure. JumpStart provides full support for it, ensuring your model runs at peak speed and efficiency.

**Strong Security and Scalability**

Running on AWS's validated infrastructure maintains data security (deployment within VPC, etc.) and flexibly scales infrastructure based on traffic, providing a seamless connection from research to production services.

***

### 3. Getting Started

#### 3.1 Search for the `NVIDIA Cosmos Reason` Model in the Model Hub

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### 3.2 Deploy the Model

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

#### 3.3 Configure Deployment Options

Set the instance type and number of instances to deploy the model

***

### References

* [**\[AWS News\]** Build Production-Ready Drug Discovery and Robotics Pipelines with NVIDIA NIMs on SageMaker JumpStart](https://aws.amazon.com/about-aws/whats-new/2026/02/accelerate-biosciences-and-robotics-with-NVIDIA-NIMs-on-sagemaker-jumpstart/)
* [**\[NVIDIA Research\]** Cosmos-Reason1](https://research.nvidia.com/labs/dir/cosmos-reason1/)
* [**\[Github\]** nvidia-cosmos/cosmos-reason1](https://github.com/nvidia-cosmos/cosmos-reason1)
* [**\[HuggingFace\]** nvidia/Cosmos-Reason1-7B](https://huggingface.co/nvidia/Cosmos-Reason1-7B)
