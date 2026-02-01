---
description: 대규모 데이터로 학습된 모델이 여러 로봇 업무를 일반화
---

# Robot Foundation Model

Robot Foundation Model (RFM)은 대규모의 다양한 데이터로 사전학습된 로봇용 파운데이션 모델로, 새로운 상황에 대해 추론하고 실제 변동성을 견고하게 처리하며 신속하게 새로운 작업을 학습할 수 있도록 설계되었습니다.

RFM은 시각(Perception), 언어(Language), 행동(Action)을 통합한 **“Perception–Reasoning–Action Loop”** 구조를 가집니다. 로봇은 환경으로부터 입력된 관측값을 해석하고(Perception), 언어적 명령이나 목표를 이해한 뒤(Language Reasoning), 행동정책(Policy Model)을 통해 실제 동작(Action)으로 연결합니다.

이 과정은 다음의 기술적 프레임워크를 기반으로 합니다.

| 구성 단계                | 주요 기술 기반                                                     | 설명                             |
| -------------------- | ------------------------------------------------------------ | ------------------------------ |
| Perception (인식)      | Vision Transformer, 3D Point Cloud Encoder, RGB-D Fusion     | 카메라 및 센서 데이터를 통해 장면과 객체 상태를 인식 |
| Reasoning (이해/계획)    | LLM 기반 Goal Parsing, Graph Transformer                       | 언어적 명령과 시각 정보를 통합하여 행동 계획 생성   |
| Action / Policy (행동) | Diffusion Policy, Reinforcement Learning, Imitation Learning | 물리 제약 내에서 최적의 행동 정책을 실행        |

#### 학습 데이터 전략

**이질적 데이터 혼합 (Heterogeneous Data Pyramid)**:

* **하단부**: 대량의 일반적 데이터 (합성 데이터, 인간 영상)
* **중간부**: 다중 로봇 교차 데이터 (Open X-Embodiment)
* **상단부**: 특정 로봇 실제 궤적 데이터

이는 "데이터 섬"(Data Islands) 문제를 해결하기 위해 설계되었습니다. 로봇의 다양한 체형, 센서, 액추에이터 자유도 차이로 인해 발생하는 호환성 문제를 극복합니다.

#### 기술적 특징

1. **교차-체현 학습 (Cross-Embodiment Learning)**: 여러 로봇 플랫폼에서 학습 가능
2. **데이터 효율성**: 소수 시연(100개 시연)으로 새 작업 학습 가능
3. **일반화 능력**: 예상하지 못한 환경과 다양한 자유로운 어휘 지시 처리
4. **체현 적응**: 완전히 새로운 로봇 체형에 빠르게 적응

<br>
