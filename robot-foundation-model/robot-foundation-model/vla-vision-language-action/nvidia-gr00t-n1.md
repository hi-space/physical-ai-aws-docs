---
description: 'GR00T N1: An Open Foundation Model for Generalist Humanoid Robots (2025)'
---

# NVIDIA GR00T N1

> _범용 로봇에는 다재다능한 신체와 지능적인 두뇌가 필요합니다._

#### 1. 핵심 주제: GR00T N1 소개

* 연구팀은 휴머노이드 로봇을 위한 개방형 파운데이션 모델인 **GR00T N1**을 소개합니다.
* 이 모델은 **VLA(Vision-Language-Action)** 모델로, 로봇이 시각(Vision)과 언어(Language)를 통해 상황을 이해하고 행동(Action)을 결정하는 시스템입니다.
* 로봇이 새로운 상황을 추론하고 현실 세계의 변수에 적응하는 능력을 크게 향상시킨 연구입니다.

#### 2. 배경: 기존 접근 방식의 한계

**로보틱스에서의 파운데이션 모델** 로보틱스를 위한 파운데이션 모델 개발 및 사용은 최근 큰 관심을 받고 있습니다. 한 가지 일반적인 접근 방식은 기존에 사전 학습된 파운데이션 모델을 저수준 로봇 특화 정책과 함께 고수준 블랙박스 추론 모듈로 활용하는 것입니다. 이 접근 방식은 로봇이 사전 학습된 파운데이션 모델을 사용하여 저수준 기술 또는 동작의 시퀀스를 계획할 수 있게 합니다.&#x20;

다른 접근 방식은 사전 학습된 파운데이션 모델을 로보틱스 데이터로 fine-tune하여 Vision-Language-Action (VLA) 모델을 구축하는 것입니다. 고수준 VLM 계획과 저수준 제어 사이에 엄격한 계층을 강제하는 대신, 이러한 VLA 모델은 다운스트림 배포 작업을 향한 end-to-end 최적화를 허용합니다.&#x20;

우리는 GR00T N1을 학습시키기 위해 유사한 접근 방식을 취하며 Eagle-2 모델을 기본 Vision Language Model (VLM)로 사용합니다. **로봇 학습을 위한 데이터셋** 로봇 학습의 핵심 과제는 범용 로봇을 학습시키는 데 필요한 대규모의 다양하고 구현된 데이터셋의 부족입니다. 일반적인 접근 방식 중 하나는 로봇 텔레오퍼레이션을 사용하는 것으로, 인간이 스마트폰이나 Virtual Reality (VR) 컨트롤러와 같은 장치를 사용하여 로봇을 제어하여 관심 작업을 수행합니다.

#### 3. 기술적 특징: 이중 시스템 아키텍처 (Dual-System Architecture)

이 모델은 두 가지 시스템이 결합된 독특한 구조를 가지고 있으며, 두 모듈은 서로 긴밀하게 연결되어 **엔드투엔드(End-to-End)** 방식으로 함께 학습됩니다.

* **시스템 2 (Vision-Language Module):** 사람의 인지 능력에 해당합니다. 시각 정보와 언어 지시사항을 통해 주변 환경을 해석하고 이해합니다.
* **시스템 1 (Diffusion Transformer Module):** 사람의 운동 신경에 해당합니다. 해석된 정보를 바탕으로 로봇의 모터가 어떻게 움직여야 할지 부드러운 행동(Action)을 실시간으로 생성합니다.

GR00T N1은 flow-matching을 사용하여 동작 생성을 학습합니다. Diffusion Transformer (DiT)는 로봇의 고유 감각 상태와 동작을 처리하며, 이는 Eagle-2 VLM 백본의 이미지 및 텍스트 토큰과 cross-attention되어 잡음 제거된 모터 동작을 출력합니다.&#x20;

**상태 및 동작 인코더** 서로 다른 로봇 embodiment의 다양한 차원의 상태와 동작을 처리하기 위해, embodiment당 MLP를 사용하여 공유 임베딩 차원으로 투영합니다. Action Encoder MLP는 잡음이 추가된 동작 벡터와 함께 diffusion timestep도 인코딩합니다. 동작은 chunk로 처리되며, 주어진 시간 t에 모델은 시간 단계 t부터 t+H-1까지의 동작 벡터를 포함하는 chunk를 사용합니다. 우리 구현에서는 H=16으로 설정했습니다.&#x20;

**Vision-Language 모듈 (System 2)** 비전 및 언어 입력 인코딩을 위해 GR00T N1은 인터넷 규모 데이터로 사전 학습된 Eagle-2 VLM을 사용합니다. 이미지는 224×224 해상도로 인코딩된 후 pixel shuffle을 거쳐 프레임당 64개의 이미지 토큰 임베딩이 생성됩니다. 정책 학습 중에는 작업에 대한 텍스트 설명과 이미지들이 VLM에 전달됩니다. 최종 레이어가 아닌 중간 레이어 LLM 임베딩을 사용하는 것이 더 빠른 추론 속도와 더 높은 다운스트림 정책 성공률을 가져왔습니다.&#x20;

**Diffusion Transformer 모듈 (System 1)** 동작 모델링을 위해 GR00T N1은 DiT의 변형을 사용합니다. 이는 adaptive layer normalization을 통한 잡음 제거 단계 조건화를 가진 transformer입니다. DiT는 self-attention과 cross-attention 블록을 번갈아 사용하며, cross-attention 블록은 VLM이 출력한 vision-language 토큰 임베딩에 조건화할 수 있게 합니다.

#### 4. 학습 데이터 (Training Data)

다양하고 방대한 데이터를 혼합하여 모델을 학습시켰습니다.

* 실제 로봇의 움직임 궤적 (Real robot trajectories)
* 사람의 행동 비디오 (Human videos)
* 시뮬레이션으로 생성된 합성 데이터 (Synthetically generated datasets)

#### 5. 연구 성과 및 결과

* **성능:** 다양한 로봇 신체 조건(Embodiments)에서 시뮬레이션 벤치마크를 테스트한 결과, 기존의 최첨단(SOTA) 모방 학습 모델들보다 뛰어난 성능을 보였습니다.
* **실제 적용:** [Fourier GR-1](https://www.fftai.com/products-gr1) 이라는 실제 휴머노이드 로봇에 이 모델을 탑재하여 테스트했습니다. 언어 명령에 따라 양손을 사용하는 작업(Bimanual manipulation tasks)을 수행했으며, 적은 데이터로도 높은 효율과 성능을 달성했습니다.

### 요약

1. 최근 휴머노이드 로봇의 발전은 인간 세계에서 범용 자율성을 구축하기 위한 하드웨어 플랫폼으로서 큰 가능성을 보여주고 있습니다
2. 대규모의 다양한 데이터 소스로 학습된 로봇 파운데이션 모델(Robot Foundation Model)은 로봇이 새로운 상황에 대해 추론하고, 실세계의 변동성을 견고하게 처리하며, 새로운 작업을 빠르게 학습할 수 있도록 하는 데 필수적입니다
3. 이를 위해 우리는 휴머노이드 로봇을 위한 오픈 파운데이션 모델인 GR00T N1을 소개합니다
4. GR00T N1은 이중 시스템 아키텍처(Dual-System Architecture)를 가진 Vision-Language-Action (VLA) 모델입니다
5. 비전-언어 모듈(System 2)은 시각과 언어 지시를 통해 환경을 해석하고, 후속 Diffusion Transformer 모듈(System 1)은 실시간으로 유연한 모터 동작을 생성합니다



***

### References

{% embed url="https://arxiv.org/pdf/2503.14734" %}

{% embed url="https://github.com/NVIDIA/Isaac-GR00T" %}

