# Table of contents

## Introduction

* [Physical AI on AWS](README.md)

## Overview

* [Physical AI란?](overview/physical-ai.md)

## Physical AI on AWS 구현 가이드 <a href="#guide" id="guide"></a>

* [Physical AI E2E 워크샵](guide/e2e-workshop/README.md)
  * [1. 클라우드 인프라 준비 및 환경 확인](guide/e2e-workshop/1.-isaaclab-infra-setup.md)
  * [2. Isaac Lab 강화학습 실행](guide/e2e-workshop/2.-isaaclab-rl-train.md)
  * [3. AWS Batch를 활용한 대규모 강화학습](guide/e2e-workshop/3.-isaaclab-rl-train-batch.md)
  * [4. IsaacSim에서 학습된 모델 로드](guide/e2e-workshop/4.-isaaclab-rl-test.md)
  * [5. VLA Fine-tuning 인프라 배포](guide/e2e-workshop/5.-train-infra-setup.md)
  * [6. AWS Batch를 활용한 VLA Fine-tuning](guide/e2e-workshop/6.-vla-train-batch.md)
  * [7. SageMaker를 활용한 VLA 학습 및 배포 파이프라인](guide/e2e-workshop/7.-vla-train-sagemaker.md)
  * [8. Closed-loop 평가](guide/e2e-workshop/8.-evaluation.md)
  * [9. Edge Operation](guide/e2e-workshop/9.-edge-operation.md)
  * [부록. Quick Wrap-up](guide/e2e-workshop/98-quick-wrapup.md)
  * [부록. 실무 팁 및 참고 사항](guide/e2e-workshop/99-tips.md)
* [RL/VLA 모델 HyperPod 분산 학습](guide/hyperpod-training/README.md)
  * [1. 인프라 배포](guide/hyperpod-training/1.-infra-deploy.md)
  * [2. 클러스터 접속 및 확인](guide/hyperpod-training/2.-cluster-access.md)
  * [3. 데이터 준비](guide/hyperpod-training/3.-data-preparation.md)
  * [4. VLA 학습](guide/hyperpod-training/4.-vla-training.md)
  * [5. RL 학습](guide/hyperpod-training/5.-rl-training.md)
  * [6. 시뮬레이션 검증](guide/hyperpod-training/6.-simulation-verification.md)
  * [7. MLflow 실험 추적](guide/hyperpod-training/7.-mlflow-tracking.md)
  * [8. 리소스 정리](guide/hyperpod-training/8.-cleanup.md)
* [NVIDIA OSMO on AWS](guide/nvidia-osmo-on-aws/README.md)
  * [1. 인프라 배포 (CDK)](guide/nvidia-osmo-on-aws/1.-infra-deploy.md)
  * [2. Kubernetes 기본 설정](guide/nvidia-osmo-on-aws/2.-eks-setup.md)
  * [3. OSMO 설치 (Helm)](guide/nvidia-osmo-on-aws/3.-osmo-install.md)
  * [4. OSMO 구성 (Credential, Config, Queue)](guide/nvidia-osmo-on-aws/4.-osmo-config.md)
  * [5. GPU 워크플로 검증](guide/nvidia-osmo-on-aws/5.-gpu-verification.md)
  * [6. GR00T Fine-tuning 실행](guide/nvidia-osmo-on-aws/6.-groot-finetune.md)
  * [7. 정리 (Cleanup)](guide/nvidia-osmo-on-aws/7.-cleanup.md)
* [NVIDIA Cosmos Predict 2.5 배포](guide/nvidia-cosmos-predict-2.5/README.md)
  * [Cosmos NIM + EKS](guide/nvidia-cosmos-predict-2.5/cosmos-nim-+-eks.md)
  * [AWS Batch + EC2](guide/nvidia-cosmos-predict-2.5/aws-batch-+-ec2.md)
* [NVIDIA Cosmos Reason-1-7B 배포](guide/nvidia-cosmos-reason-1-7b.md)

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

## 참고 자료 <a href="#references" id="references"></a>

* [AWS의 Physical AI 관련 문서](references/aws-physical-ai.md)
