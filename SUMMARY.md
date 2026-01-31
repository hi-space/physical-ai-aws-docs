# Table of contents

## Introduction

* [Physical AI on AWS](README.md)

## Overview

* [Physical AI란?](overview/physical-ai.md)
* [왜 지금 Physical AI인가?](overview/physical-ai-1.md)
* [Physical AI 구현에 필요한 기술](overview/physical-ai-2.md)
* [Physical AI의 실제 산업 적용 사례](overview/physical-ai-3.md)
* [왜 AWS에서 Physical AI를 할까?](overview/aws-physical-ai.md)
* [TBD](overview/tbd/README.md)
  * [1. 데이터 (Data)](overview/tbd/1.-data/README.md)
    * [로봇 데이터 수집 파이프라인](overview/tbd/1.-data/undefined.md)
    * [데이터 전처리 / 라벨링](overview/tbd/1.-data/undefined-1.md)
    * [모범 사례](overview/tbd/1.-data/undefined-2.md)
  * [2. 학습 (Training)](overview/tbd/2.-training/README.md)
    * [학습 방법](overview/tbd/2.-training/undefined.md)
    * [학습 환경 구축](overview/tbd/2.-training/undefined-1.md)
    * [NVIDIA GR00T N1](overview/tbd/2.-training/nvidia-gr00t-n1.md)
  * [3. 시뮬레이션 (Simulation)](overview/tbd/3.-simulation/README.md)
    * [Isaac Sim 환경 구축](overview/tbd/3.-simulation/isaac-sim.md)
    * [대규모 시뮬레이션](overview/tbd/3.-simulation/undefined.md)
    * [NVIDIA Cosmos WFM on Amazon EKS](overview/tbd/3.-simulation/nvidia-cosmos-wfm-on-amazon-eks.md)
  * [4. Sim2Real](overview/tbd/4.-sim2real.md)
  * [5. 오케스트레이션 (Agentic Orchestration)](overview/tbd/5.-agentic-orchestration.md)

## Physical AI on AWS 구현 가이드 <a href="#physical-ai-on-aws-guide" id="physical-ai-on-aws-guide"></a>

* [NVIDIA Isaac Lab on AWS](physical-ai-on-aws-guide/nvidia-isaac-lab-on-aws/README.md)
  * [1. GPU 기반 클라우드 인프라 준비](physical-ai-on-aws-guide/nvidia-isaac-lab-on-aws/1.-gpu.md)
  * [2. Isaac Sim / Isaac Lab 환경 확인](physical-ai-on-aws-guide/nvidia-isaac-lab-on-aws/2.-isaac-sim-isaac-lab.md)
  * [3. AWS Batch를 활용한 대규모 학습](physical-ai-on-aws-guide/nvidia-isaac-lab-on-aws/3.-aws-batch.md)
  * [4. IsaacSim에서 학습된 모델 로드](physical-ai-on-aws-guide/nvidia-isaac-lab-on-aws/4.-isaacsim.md)

## Robot Foundation Model

* [World Foundation Model](robot-foundation-model/world-foundation-model/README.md)
  * [NVIDIA Cosmos World Foundation Model](robot-foundation-model/world-foundation-model/nvidia-cosmos-world-foundation-model.md)
* [Robot Foundation Model](robot-foundation-model/robot-foundation-model.md)
* [VLA (Vision-Language-Action) 모델](robot-foundation-model/vla-vision-language-action/README.md)
  * [DeepMind RT-2](robot-foundation-model/vla-vision-language-action/deepmind-rt-2.md)
  * [OpenVLA](robot-foundation-model/vla-vision-language-action/openvla.md)
  * [Gemini Robotics](robot-foundation-model/vla-vision-language-action/gemini-robotics.md)
  * [NVIDIA GR00T N1](robot-foundation-model/vla-vision-language-action/nvidia-gr00t-n1.md)

## Architecture & Best Practice

* [End-to-end Architecture](architecture-and-best-practice/end-to-end-architecture.md)

## 참고 자료

* [AWS의 Physical AI 관련 문서](undefined/aws-physical-ai.md)
