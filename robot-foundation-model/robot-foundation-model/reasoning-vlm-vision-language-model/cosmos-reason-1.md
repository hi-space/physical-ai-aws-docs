---
description: 'Cosmos-Reason1: From Physical Common Sense To Embodied Reasoning (2025)'
---

# Cosmos-Reason 1

### 들어가며

AI가 점점 똑똑해지고 있습니다. GPT-4는 코딩 문제를 풀고, Claude는 복잡한 논리 퍼즐을 해결하며, DeepSeek-R1은 수학 경시 문제에서 인간을 능가합니다. 하지만 이런 모델들에게 "물이 담긴 컵을 들고 계단을 내려가라"는 간단한 작업을 시키면 어떨까요? 놀랍게도, 텍스트 기반 추론에서는 천재적인 이런 모델들이 물리 세계와의 상호작용에서는 기본적인 상식조차 결여하고 있습니다.

기존 대형 언어 모델(LLM)들은 방대한 인터넷 텍스트 데이터로 학습되었지만, 실제 물리 세계가 어떻게 작동하는지에 대한 직관적 이해가 부족합니다. 중력이 물체에 어떻게 작용하는지, 물체가 다른 물체와 어떻게 상호작용하는지, 로봇 팔이 물건을 집을 때 무슨 일이 일어나는지를 진정으로 이해하지 못합니다. 이는 Physical AI 시스템 개발에 큰 걸림돌이 됩니다.

오늘 소개할 논문 "Cosmos-Reason1"은 NVIDIA가 이 문제를 해결하기 위해 제시한 해법입니다. Cosmos-Reason1은 비디오를 통해 물리 세계를 인식하고, 긴 chain-of-thought 추론 과정을 거쳐 물리적으로 타당한 답변과 embodied 의사결정을 생성하는 멀티모달 대형 언어 모델입니다. 이 글에서는 Cosmos-Reason1의 핵심 아이디어, 학습 방법론, 그리고 실제 성능을 자세히 살펴보겠습니다.

***

### Physical AI가 필요한 이유

#### Physical AI란 무엇인가?

Physical AI는 물리 세계와 상호작용하도록 설계된 AI 시스템을 의미합니다. 로봇, 자율주행 차량, 드론 등이 여기에 해당합니다. 이러한 시스템들은 단순히 정보를 처리하는 것을 넘어서 실제 환경에서 움직이고, 물체를 조작하며, 안전하게 작업을 수행해야 합니다.

전통적인 AI 모델들이 탁월한 성과를 보이는 영역—수학, 코딩, 텍스트 생성—은 모두 구조화되고 결정론적인 환경입니다. 수학 문제는 명확한 정답이 있고, 코드는 논리적으로 검증 가능합니다. 하지만 물리 세계는 다릅니다. 불확실하고, 동적이며, 복잡한 물리 법칙에 지배됩니다.

#### 기존 접근법의 한계

초기 연구들은 기존 LLM이나 VLM(Vision-Language Model)을 로봇 시스템에 그대로 적용하려 했습니다. 예를 들어:

1. **Zero-shot Task Planning**: LLM에게 "식탁을 차리세요"라고 하면, LLM이 자연어로 계획을 생성합니다. 하지만 이런 계획들은 물리적 제약을 고려하지 않아 실행 불가능한 경우가 많았습니다.
2. **Code as Policies**: LLM이 로봇 제어 코드를 직접 생성하는 방식입니다. 그러나 LLM은 물리 세계에 대한 직관이 부족해 비현실적인 동작을 생성하곤 했습니다.

이런 한계는 근본적인 문제에서 비롯됩니다: **모델이 물리적 상식(physical common sense)을 갖고 있지 않다는 것입니다.**

***

### Cosmos-Reason1의 핵심 아이디어

#### 두 가지 필수 능력 정의

NVIDIA 연구팀은 Physical AI를 위해 두 가지 핵심 능력을 정의했습니다:

**1. Physical Common Sense (물리적 상식)**

물리적 상식은 환경에 대한 일반적이고 embodiment에 구애받지 않는 이해를 의미합니다. 마치 어린 아이가 생후 몇 개월 만에 물체 영속성(object permanence)과 중력을 이해하는 것처럼, AI도 수동적 관찰을 통해 물리 세계가 어떻게 작동하는지 배워야 합니다.

연구팀은 물리적 상식을 세 가지 주요 범주로 나누는 계층적 온톨로지(ontology)를 제안했습니다:

* **Space (공간)**: 물체 간의 관계, 상호작용, 환경 (예: 컵이 책상 위에 있다, 공이 상자 안에 들어갈 수 있다)
* **Time (시간)**: 시간에 따라 전개되는 행동과 사건 (예: 사건의 순서, 인과관계, 계획)
* **Fundamental Physics (기본 물리)**: 물체의 속성과 핵심 물리 법칙 (예: 역학, 전자기학, 열역학)

이 세 범주는 다시 16개의 세부 하위 범주로 나뉩니다. 예를 들어, Space는 Relationship(관계), Plausibility(타당성), Affordance(행위가능성), Environment(환경)로 세분화됩니다.

**2. Embodied Reasoning (체화된 추론)**

Embodied reasoning은 물리 세계와 실제로 상호작용하는 능력입니다. 단순히 이해하는 것을 넘어, 행동을 계획하고 실행해야 합니다. 연구팀은 네 가지 핵심 능력을 정의했습니다:

* **복잡한 감각 입력 처리**: 불완전하고 애매한 시각 정보에서 의미 있는 패턴 추출
* **행동 효과 예측**: "로봇 팔이 이 물체를 잡으면 어떻게 될까?" 같은 인과 관계 이해
* **물리적 제약 존중**: 관성, 마찰, 재료 특성 등 실제 물리 법칙 고려
* **상호작용을 통한 학습**: 피드백을 통해 행동을 동적으로 개선

### 모델 아키텍처

#### 멀티모달 구조

Cosmos-Reason1은 decoder-only 아키텍처를 채택했습니다. 이는 LLaVA와 유사한 방식으로, 모든 모달리티(이미지, 비디오, 텍스트)를 통합된 방식으로 처리합니다.

구체적인 구조는 다음과 같습니다:

1. **Vision Encoder**: InternViT-300M을 사용해 비디오 프레임을 처리합니다. 각 448×448 프레임은 1,024개의 시각 토큰으로 변환됩니다.
2. **Projector**: 2×2 PixelShuffle 다운샘플링을 통해 시각 토큰을 256개로 줄이고, 2-layer MLP로 텍스트 토큰 임베딩 공간에 정렬합니다.
3. **LLM Backbone**: Cosmos-Reason1-7B는 Qwen2.5-VL 기반의 dense Transformer를, 56B는 Nemotron-H 기반의 hybrid Mamba-MLP-Transformer를 사용합니다.

#### Hybrid Mamba-MLP-Transformer의 장점

Transformer는 self-attention 메커니즘 때문에 시퀀스 길이의 제곱에 비례하는 계산 복잡도(O(n²))를 가집니다. 긴 비디오를 처리할 때 이는 치명적인 병목이 됩니다.

Mamba 아키텍처는 selective state space model을 활용해 선형 복잡도(O(n))를 달성합니다. "상태(state)"를 유지하며 시퀀스를 순차적으로 처리하되, 중요한 정보는 상태에 유지하고 덜 중요한 정보는 잊어버리도록 학습합니다. 마치 사람이 대화에서 중요한 부분은 기억하고 사소한 부분은 흘려듣는 것과 유사합니다.

하지만 Mamba만으로는 모든 디테일을 포착하기 어렵습니다. 따라서 Cosmos-Reason1-56B는 Mamba layer와 Transformer layer를 혼합한 hybrid 아키텍처를 사용합니다. 118개 layer 중 대부분은 효율적인 Mamba layer로, 일부만 Transformer layer로 구성하여 효율성과 성능의 균형을 맞췄습니다.

### 학습 방법론

Cosmos-Reason1은 두 단계로 학습됩니다: **Physical AI SFT(Supervised Fine-Tuning)**&#xC640; **Physical AI RL(Reinforcement Learning)**&#xC785;니다.

#### Stage 1: Physical AI SFT

SFT 단계에서는 약 400만 개의 비디오-텍스트 쌍을 큐레이션했습니다. 이 데이터는 다음을 포함합니다:

**Physical Common Sense 데이터**

1. **Free-form Questions**: 9.9K개 비디오에서 99K개의 이해(understanding) 질문과 59.4K개의 추론(reasoning) 질문 생성
2. **Multiple Choice Questions**: 1.2M개 비디오에서 2.4M개 이해 질문과 600K개 추론 질문 생성

데이터 생성 파이프라인:

* 고품질 비디오 큐레이션 (인간 선호도 기반)
* 상세한 캡션 작성 (인간 또는 VLM 사용)
* LLM을 프롬프트하여 질문 생성 (이해용 vs. 추론용 구분)
* DeepSeek-R1을 사용해 추론 trace 추출
* 규칙 기반 정제 및 재작성

**Embodied Reasoning 데이터**

연구팀은 다양한 embodiment에서 데이터를 수집했습니다:

* **BridgeData V2**: 로봇 팔 조작 (129.2K 이해 + 129.1K 추론)
* **RoboVQA**: 로봇 VQA 데이터셋 (218.5K 이해 + 920K 추론)
* **AgiBot**: 휴머노이드 로봇 (19.4K 각)
* **HoloAssist**: 1인칭 시점 인간 행동 (136.3K 각)
* **AV**: 자율주행 차량 (12.4K 각)

세 가지 핵심 속성에 초점을 맞췄습니다:

1. Task-completion verification (작업 완료 여부 판단)
2. Action affordance (행동 가능성 평가)
3. Next plausible action prediction (다음 행동 예측)

**Intuitive Physics 데이터**

자기 지도 학습으로 생성 가능한 데이터:

1. **Spatial Puzzles**: 이미지를 2×2 패치로 나누고 섞어서, 올바른 위치를 추론하도록 학습 (11K 샘플)
2. **Arrow of Time (AoT)**: 비디오를 정방향/역방향으로 재생하고, 물리 법칙에 따라 어느 것이 올바른지 판단 (30K 샘플)
3. **Object Permanence**: 시뮬레이션 환경에서 카메라가 움직일 때 물체가 예상치 않게 사라지는지 감지 (10K 샘플)

#### Stage 2: Physical AI RL

수학이나 코딩과 달리 Physical AI에서는 정답이 명확하지 않습니다. "다음 행동은 무엇인가?"라는 질문에는 여러 타당한 답이 있을 수 있습니다. 이를 해결하기 위해 연구팀은 \*\*규칙 기반의 검증 가능한 보상(rule-based verifiable rewards)\*\*을 설계했습니다.

**보상 설계**

1. **Accuracy Reward**: Multiple choice question에서 정답을 맞췄는지 문자열 매칭으로 검증
2. **Format Reward**: 추론 과정을 `<think></think>` 태그로, 답변을 `<answer></answer>` 태그로 감싸는지 확인

**데이터 구성**

* Physical Common Sense: 5,133 MCQ (easy/hard subset으로 구분)
* Embodied Reasoning: 각 데이터 소스에서 200-250 샘플
* Intuitive Physics: 24,079 샘플 (가장 확장 가능)

**학습 설정**

* 알고리즘: GRPO (Group Relative Policy Optimization)
* 각 질문당 9개 응답 샘플링
* KL penalty coefficient: 0.005
* 500 iteration 학습

연구팀은 완전히 비동기적이고 내결함성을 가진 RL 학습 프레임워크를 개발했습니다. 기존 colocated 프레임워크 대비 약 160% 효율 향상을 달성했습니다.

***

### 벤치마크 구축

공정한 평가를 위해 연구팀은 새로운 벤치마크를 구축했습니다.

#### Physical Common Sense 벤치마크

426개 비디오에서 604개 질문을 수작업으로 큐레이션:

* Space: 80개 (13.25%)
* Time: 298개 (49.33%)
* Fundamental Physics: 226개 (37.4%)

#### Embodied Reasoning 벤치마크

610개 질문을 6개 데이터셋에서 수집:

* BridgeData V2, RoboVQA, RoboFail: 로봇 팔
* AgiBot: 휴머노이드 로봇
* HoloAssist: 인간 (1인칭 시점)
* AV: 자율주행 차량

각 100개 질문으로 구성되며, 모두 MCQ 형식입니다.

#### Intuitive Physics 벤치마크

각 작업당 100개 질문:

* Arrow of Time
* Spatial Puzzle
* Object Permanence

### 실험 결과

#### Physical Common Sense 성능

| 모델                     | Space | Time | Physics | 평균              |
| ---------------------- | ----- | ---- | ------- | --------------- |
| Gemini 2.0 Flash       | 53.8  | 50.0 | 46.9    | 50.2            |
| GPT-4o                 | 61.3  | 54.7 | 50.9    | 55.6            |
| OpenAI o1              | 63.8  | 58.1 | 58.0    | 59.9            |
| Qwen2.5-VL-7B          | 48.8  | 56.4 | 37.2    | 47.4            |
| **Cosmos-Reason1-7B**  | 54.2  | 58.7 | 50.0    | **54.3** (+6.9) |
| **Cosmos-Reason1-56B** | 61.3  | 65.5 | 53.9    | **60.2** (+2.0) |

Cosmos-Reason1-7B는 기반 모델(Qwen2.5-VL) 대비 6.9% 향상, 56B는 2.0% 향상을 보였습니다. 56B 모델은 OpenAI o1을 근소하게 앞섰습니다.

#### Embodied Reasoning 성능

| 모델                     | Bridge | RoboVQA | AgiBot | HoloAssist | AV   | RoboFail | 평균               |
| ---------------------- | ------ | ------- | ------ | ---------- | ---- | -------- | ---------------- |
| GPT-4o                 | 42.0   | 71.8    | 32.0   | 65.0       | 46.0 | 63.0     | 53.3             |
| OpenAI o1              | 42.0   | 80.0    | 44.0   | 63.0       | 37.0 | 61.0     | 54.5             |
| **Cosmos-Reason1-7B**  | 58.8   | 83.8    | 49.4   | 63.0       | 55.6 | 60.0     | **61.8** (+11.0) |
| **Cosmos-Reason1-56B** | 65.0   | 80.0    | 47.6   | 57.8       | 65.8 | 66.2     | **63.7** (+10.2) |

두 모델 모두 기반 VLM 대비 10% 이상의 대폭적인 성능 향상을 보였습니다. 특히 BridgeData V2(로봇 팔 조작)와 AV(자율주행)에서 두드러진 개선이 있었습니다.

#### Intuitive Physics 성능: 기존 모델의 충격적인 실패

| 모델                    | AoT  | Spatial Puzzle | Object Permanence | 평균               |
| --------------------- | ---- | -------------- | ----------------- | ---------------- |
| Random Guess          | 50.0 | 25.0           | 50.0              | 41.7             |
| Gemini 2.0 Flash      | 50.0 | 31.0           | 48.0              | 43.0             |
| GPT-4o                | 50.0 | 77.0           | 48.0              | 58.3             |
| OpenAI o1             | 51.0 | 64.0           | 49.0              | 54.7             |
| **Cosmos-Reason1-7B** | 56.0 | 85.4           | 82.0              | **74.5** (+32.4) |

놀랍게도, MMMU 같은 표준 벤치마크에서 우수한 성능을 보이는 GPT-4o와 OpenAI o1조차 Arrow of Time과 Object Permanence에서는 **무작위 추측 수준**입니다. 이는 기존 VLM들이 물리 세계에 대한 진정한 이해가 부족함을 보여줍니다.

Cosmos-Reason1-7B는 SFT만으로도 평균 74.5%를 달성하며, 32.4%p의 엄청난 개선을 보였습니다.

#### Physical AI RL의 추가 개선

| 벤치마크              | SFT  | +RL  | 개선   |
| ----------------- | ---- | ---- | ---- |
| Common Sense      | 54.3 | 56.2 | +1.9 |
| Embodied (평균)     | 60.7 | 65.7 | +5.0 |
| Intuitive Physics | 74.5 | 81.5 | +7.0 |

RL 학습은 모든 벤치마크에서 추가적인 성능 향상을 가져왔습니다. 특히 Intuitive Physics에서 7.0%p의 큰 개선이 있었습니다.

**흥미로운 발견**: RL 이후 모델은 애매한 질문에 대해 제공된 선택지를 모두 거부하는 법을 학습했습니다. 예를 들어, 자율주행 시나리오에서 제시된 모든 선택지가 물리적으로 불가능한 경우, 모델은 각 선택지를 신중히 평가한 후 "none"이라고 답하며 보수적인 판단을 내렸습니다.

***

### 핵심 기여와 시사점

#### 1. 체계적인 Ontology 제안

Cosmos-Reason1은 Physical AI 능력을 체계적으로 정의한 최초의 시도입니다:

* 16개 세부 범주로 구성된 physical common sense ontology
* 능력 × embodiment 2차원 embodied reasoning ontology

이 프레임워크는 향후 Physical AI 연구의 표준이 될 수 있습니다.

#### 2. Rule-based Verifiable Rewards for Physical AI

수학/코딩에서 성공한 RL 접근법을 Physical AI에 적용하는 것은 어려웠습니다. 정답이 명확하지 않기 때문입니다. 연구팀은 MCQ와 자기 지도 학습 작업을 통해 이 문제를 우회했습니다.

특히 Intuitive Physics 데이터는 확장 가능하다는 점이 중요합니다:

* Spatial Puzzle: 어떤 이미지든 적용 가능
* Arrow of Time: 단순히 비디오를 역재생하면 됨
* Object Permanence: 시뮬레이션 환경에서 쉽게 생성

#### 3. 하드웨어 효율적인 RL 프레임워크

완전히 비동기적인 RL 프레임워크로 기존 대비 160% 효율 향상을 달성했습니다. 더불어 노드 장애 시에도 자동으로 재구성하여 학습을 계속하는 내결함성을 갖췄습니다.

#### 4. 기존 모델의 치명적 약점 발견

GPT-4o, Gemini, OpenAI o1 같은 최첨단 모델들이 Arrow of Time과 Object Permanence에서 무작위 추측 수준이라는 발견은 충격적입니다. 이는 현재 벤치마크들이 모델의 물리 세계 이해를 제대로 측정하지 못함을 시사합니다.

***

### 실용적 활용 가능성

Cosmos-Reason1은 다음 분야에서 즉시 활용 가능합니다:

**1. 로봇 공학**

* 제조업 로봇: 복잡한 조립 작업에서 다음 단계 예측
* 서비스 로봇: 가정이나 병원에서 물체 조작
* 휴머노이드 로봇: 인간과 유사한 작업 수행

**2. 자율주행**

* 복잡한 도로 상황에서 안전한 의사결정
* 예측하기 어려운 보행자/차량 행동 대응
* 극한 날씨 조건에서의 주행

**3. AR/VR**

* 물리적으로 타당한 가상 환경 생성
* 사용자 행동에 대한 현실적인 반응 시뮬레이션

**4. 시뮬레이션 및 훈련**

* 로봇 정책을 실제 배포 전에 시뮬레이션에서 검증
* 위험한 시나리오에서 인간 훈련 (예: 우주 비행사, 구조대원)

***

### 한계점과 개선 방향

#### 현재 한계

**1. 일부 작업에서 Transformer 대비 낮은 성능**

* 특정 벤치마크에서 0.5-1% 낮은 성능
* 통계적으로 유의미한 수준은 아님

**2. 생태계 부족**

* 새로운 아키텍처이므로 pre-trained model, library 부족
* 커뮤니티 지원이 Transformer 대비 약함

**3. RoboFail 벤치마크 정체**

* 매우 어려운 affordance 시나리오에서 개선 없음
* 더 대표적인 학습 데이터 필요

**4. 짧은 시퀀스에서 비효율**

* 512 토큰 미만에서는 Transformer보다 느릴 수 있음
* 하지만 Physical AI는 일반적으로 긴 컨텍스트 필요

#### 향후 연구 방향

**1. 상호작용을 통한 학습**

* 현재는 수동적 관찰만 다룸
* 능동적 탐험과 피드백 학습 추가 필요

**2. 멀티모달 확장**

* 현재는 비디오 중심
* 촉각, 청각, 힘 센서 등 다양한 센서 통합

**3. Transformer와의 하이브리드 최적화**

* 어떤 layer를 Mamba로, 어떤 layer를 Transformer로?
* 작업별 최적 비율 탐구

**4. 더 긴 시퀀스 처리**

* 현재 32 프레임 (최대 16초)
* 전체 에피소드(수분\~수시간) 처리 능력 개발

**5. 실시간 추론 최적화**

* 로봇은 즉각적인 의사결정 필요
* 추론 속도와 품질의 트레이드오프 연구

***

### 결론

Cosmos-Reason1은 Physical AI를 위한 추론 모델 개발에서 중요한 이정표입니다. 세 가지 핵심 성과를 달성했습니다:

**1. 체계적 프레임워크**

* Physical common sense와 embodied reasoning을 명확히 정의
* 향후 연구를 위한 ontology 제공

**2. 대폭적인 성능 향상**

* 기반 VLM 대비 common sense에서 +6.9%, embodied reasoning에서 +11.0%
* RL로 추가 +5.0% 향상

**3. 기존 모델의 약점 발견**

* 최첨단 VLM들도 기본 물리 추론에서 실패
* Intuitive Physics에서 +32.4% 개선으로 이 격차 해소

**핵심 포인트**

* **물리적 상식이 중요하다**: 텍스트 추론 능력만으로는 Physical AI 구축 불가
* **검증 가능한 보상 설계 가능**: MCQ와 자기 지도 학습으로 RL 적용 가능
* **확장 가능한 데이터**: Intuitive Physics는 거의 무한대로 생성 가능
* **아직 갈 길이 멀다**: RoboFail 같은 어려운 시나리오에서는 여전히 개선 필요

Cosmos-Reason1은 Physical AI 분야의 새로운 가능성을 열었습니다. 코드와 모델 가중치를 NVIDIA Open Model License 하에 공개하여, 커뮤니티가 이 연구를 발전시킬 수 있도록 했습니다. 앞으로 Physical AI 시스템이 인간처럼 물리 세계를 이해하고 상호작용하는 날이 점점 가까워지고 있습니다.

***

### **참고 자료**

* **논문**: [https://arxiv.org/pdf/2503.15558](https://arxiv.org/pdf/2503.15558)
* **코드**: [https://github.com/nvidia-cosmos/cosmos-reason1](https://github.com/nvidia-cosmos/cosmos-reason1)



\#PhysicalAI #EmbodiedAI #MultimodalLLM #NVIDIA #Robotics #AutonomousVehicles #MachineLearning #DeepLearning #ReinforcementLearning #ComputerVision
