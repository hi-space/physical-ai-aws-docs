# Table of contents

## Introduction

* [Physical AI on AWS](README.md)

## Overview

* [What is Physical AI?](overview/physical-ai.md)

## Physical AI on AWS Implementation Guide <a href="#guide" id="guide"></a>

* [Physical AI E2E Workshop](guide/e2e-workshop/README.md)
  * [1. Cloud Infrastructure Setup and Environment Verification](guide/e2e-workshop/1.-isaaclab-infra-setup.md)
  * [2. Running Reinforcement Learning in Isaac Lab](guide/e2e-workshop/2.-isaaclab-rl-train.md)
  * [3. Large-scale Reinforcement Learning with AWS Batch](guide/e2e-workshop/3.-isaaclab-rl-train-batch.md)
  * [4. Loading Trained Models in IsaacSim](guide/e2e-workshop/4.-isaaclab-rl-test.md)
  * [5. VLA Fine-tuning Infrastructure Deployment](guide/e2e-workshop/5.-train-infra-setup.md)
  * [6. VLA Fine-tuning with AWS Batch](guide/e2e-workshop/6.-vla-train-batch.md)
  * [7. VLA Training and Deployment Pipeline with SageMaker](guide/e2e-workshop/7.-vla-train-sagemaker.md)
  * [8. Closed-loop Evaluation](guide/e2e-workshop/8.-evaluation.md)
  * [9. Edge Operation](guide/e2e-workshop/9.-edge-operation.md)
  * [Appendix. Quick Wrap-up](guide/e2e-workshop/98-quick-wrapup.md)
  * [Appendix. Practical Tips and References](guide/e2e-workshop/99-tips.md)
* [Distributed Training of RL/VLA Models on HyperPod](guide/hyperpod-training/README.md)
  * [1. Infrastructure Deployment](guide/hyperpod-training/1.-infra-deploy.md)
  * [2. Cluster Access and Verification](guide/hyperpod-training/2.-cluster-access.md)
  * [3. Data Preparation](guide/hyperpod-training/3.-data-preparation.md)
  * [4. VLA Training](guide/hyperpod-training/4.-vla-training.md)
  * [5. RL Training](guide/hyperpod-training/5.-rl-training.md)
  * [6. Simulation Verification](guide/hyperpod-training/6.-simulation-verification.md)
  * [7. MLflow Experiment Tracking](guide/hyperpod-training/7.-mlflow-tracking.md)
  * [8. Resource Cleanup](guide/hyperpod-training/8.-cleanup.md)
* [NVIDIA OSMO on AWS](guide/nvidia-osmo-on-aws/README.md)
  * [1. Infrastructure Deployment (CDK)](guide/nvidia-osmo-on-aws/1.-infra-deploy.md)
  * [2. Kubernetes Basic Configuration](guide/nvidia-osmo-on-aws/2.-eks-setup.md)
  * [3. OSMO Installation (Helm)](guide/nvidia-osmo-on-aws/3.-osmo-install.md)
  * [4. OSMO Configuration (Credential, Config, Queue)](guide/nvidia-osmo-on-aws/4.-osmo-config.md)
  * [5. GPU Workflow Verification](guide/nvidia-osmo-on-aws/5.-gpu-verification.md)
  * [6. Running GR00T Fine-tuning](guide/nvidia-osmo-on-aws/6.-groot-finetune.md)
  * [7. Cleanup](guide/nvidia-osmo-on-aws/7.-cleanup.md)
* [NVIDIA Cosmos Predict 2.5 Deployment](guide/nvidia-cosmos-predict-2.5/README.md)
  * [Cosmos NIM + EKS](guide/nvidia-cosmos-predict-2.5/cosmos-nim-+-eks.md)
  * [AWS Batch + EC2](guide/nvidia-cosmos-predict-2.5/aws-batch-+-ec2.md)
* [NVIDIA Cosmos Reason-1-7B Deployment](guide/nvidia-cosmos-reason-1-7b.md)

## Paper Review (TBD)

* [World Foundation Model](paper-review-tbd/world-foundation-model/README.md)
  * [Cosmos-Predict1](paper-review-tbd/world-foundation-model/cosmos-predict1.md)
  * [Cosmos-Predict2.5](paper-review-tbd/world-foundation-model/cosmos-predict2.5.md)
* [Robot Foundation Model](paper-review-tbd/robot-foundation-model/README.md)
  * [Reasoning VLM (Vision-Language Model)](paper-review-tbd/robot-foundation-model/reasoning-vlm-vision-language-model/README.md)
    * [Cosmos-Reason 1](paper-review-tbd/robot-foundation-model/reasoning-vlm-vision-language-model/cosmos-reason-1.md)
  * [VLA (Vision-Language-Action)](paper-review-tbd/robot-foundation-model/vla-vision-language-action/README.md)
    * [DeepMind RT-2](paper-review-tbd/robot-foundation-model/vla-vision-language-action/deepmind-rt-2.md)
    * [OpenVLA](paper-review-tbd/robot-foundation-model/vla-vision-language-action/openvla.md)
    * [Gemini Robotics](paper-review-tbd/robot-foundation-model/vla-vision-language-action/gemini-robotics.md)
    * [NVIDIA GR00T N1](paper-review-tbd/robot-foundation-model/vla-vision-language-action/nvidia-gr00t-n1.md)
    * [π ∗ 0.6 : a VLA That Learns From Experience](paper-review-tbd/robot-foundation-model/vla-vision-language-action/p-0.6-a-vla-that-learns-from-experience.md)

## Architecture & Best Practice

* [End-to-end Architecture](architecture-and-best-practice/end-to-end-architecture.md)

## References <a href="#references" id="references"></a>

* [AWS Physical AI Documentation](references/aws-physical-ai.md)
