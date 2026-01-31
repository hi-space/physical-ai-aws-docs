---
description: 'GR00T N1: An Open Foundation Model for Generalist Humanoid Robots (2025)'
---

# NVIDIA GR00T N1

#### 1. 핵심 주제: GR00T N1 소개

* 연구팀은 휴머노이드 로봇을 위한 개방형 파운데이션 모델인 **GR00T N1**을 소개합니다.
* 이 모델은 **VLA(Vision-Language-Action)** 모델로, 로봇이 시각(Vision)과 언어(Language)를 통해 상황을 이해하고 행동(Action)을 결정하는 시스템입니다.
* 로봇이 새로운 상황을 추론하고 현실 세계의 변수에 적응하는 능력을 크게 향상시킨 연구입니다.

#### 2. 기술적 특징: 이중 시스템 아키텍처 (Dual-System Architecture)

이 모델은 두 가지 시스템이 결합된 독특한 구조를 가지고 있으며, 두 모듈은 서로 긴밀하게 연결되어 **엔드투엔드(End-to-End)** 방식으로 함께 학습됩니다.

* **시스템 2 (Vision-Language Module):** 사람의 인지 능력에 해당합니다. 시각 정보와 언어 지시사항을 통해 주변 환경을 해석하고 이해합니다.
* **시스템 1 (Diffusion Transformer Module):** 사람의 운동 신경에 해당합니다. 해석된 정보를 바탕으로 로봇의 모터가 어떻게 움직여야 할지 부드러운 행동(Action)을 실시간으로 생성합니다.

#### 3. 학습 데이터 (Training Data)

다양하고 방대한 데이터를 혼합하여 모델을 학습시켰습니다.

* 실제 로봇의 움직임 궤적 (Real robot trajectories)
* 사람의 행동 비디오 (Human videos)
* 시뮬레이션으로 생성된 합성 데이터 (Synthetically generated datasets)

#### 4. 연구 성과 및 결과

* **성능:** 다양한 로봇 신체 조건(Embodiments)에서 시뮬레이션 벤치마크를 테스트한 결과, 기존의 최첨단(SOTA) 모방 학습 모델들보다 뛰어난 성능을 보였습니다.
* **실제 적용:** [Fourier GR-1](https://www.fftai.com/products-gr1) 이라는 실제 휴머노이드 로봇에 이 모델을 탑재하여 테스트했습니다. 언어 명령에 따라 양손을 사용하는 작업(Bimanual manipulation tasks)을 수행했으며, 적은 데이터로도 높은 효율과 성능을 달성했습니다.



***

### References

{% embed url="https://arxiv.org/pdf/2503.14734" %}

{% embed url="https://github.com/NVIDIA/Isaac-GR00T" %}

