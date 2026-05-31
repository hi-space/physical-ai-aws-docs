---
description: SageMaker JumpStart를 활용해 Cosmos Reason-1-7B 모델 배포
---

# NVIDIA Cosmos Reason-1-7B 배포

### 1. Cosmos-Reason1

Cosmos Reason은 물리 AI와 로보틱스를 위해 설계된 오픈형 **Reasoning Vision Language Model (VLM)** 입니다. 이 모델은 단순히 이미지를 인식하는 것을 넘어, **공간(Space)**, **시간(Time)**, **물리학(Physics)**&#xC744; 이해합니다.

마치 인간처럼 사전 지식과 상식을 활용해 상황을 판단하고, 구체화된 에이전트(Embodied Agent)가 다음에 취해야 할 행동을 계획(Planning)할 수 있습니다.

#### 아키텍처 및 원리

Cosmos Reason은 정교한 **생각의 사슬(Chain-of-Thought)** 추론 능력을 통해 복잡한 물리적 역학을 이해합니다.

1. **입력 처리**: 비디오나 이미지가 입력되면 **비전 인코더(Vision Encoder)**&#xC640; 특수 변환기인 **프로젝터(Projector)**&#xB97C; 거쳐 토큰으로 변환됩니다.
2. **통합 분석**: 변환된 비디오 토큰은 텍스트 프롬프트와 결합되어 핵심 모델로 전달됩니다.
3. **심층 추론**: LLM 모듈과 다양한 기법이 혼합된 코어 모델이 단계별(Step-by-step)로 사고하며, 논리적이고 상세한 응답을 생성합니다.

특히 이 모델은 물리적 상식과 엠바디드 추론 데이터로 Post-training 되었으며, 지도 미세 조정(SFT)과 강화 학습(RL)을 거쳐 인간의 레이블링 없이도 세계의 역학을 이해하도록 설계되었습니다.

#### 주요 활용 사례

Cosmos Reason은 물리적 세계의 다양한 '롱테일(Long tail)' 시나리오를 처리하는 데 탁월하며, 다음과 같은 분야에 혁신을 가져옵니다

**로봇 계획 및 추론 (Robot Planning and Reasoning)**

로봇을 위한 VLA(Vision Language Action) 모델의 두뇌 역할을 수행합니다. 휴머노이드나 자율 주행 로봇이 낯선 환경에서도 복잡한 명령을 받으면, 상식을 활용해 이를 세부 작업(Task)으로 쪼개고 실행 계획을 수립합니다.

**비디오 분석 AI 에이전트 (Video Analytics AI Agents)**

방대한 비디오 데이터에서 가치 있는 통찰력을 추출하고, 근본 원인 분석(Root-cause analysis)을 수행합니다. 도시 관제나 산업 현장의 녹화 영상 또는 라이브 스트림을 분석하여 사고의 원인이나 흐름을 논리적으로 파악합니다.

**데이터 큐레이션 및 주석 (Data Curation and Annotation)**

개발자가 대규모의 다양한 훈련 데이터셋을 구축할 때, 고품질의 큐레이션과 주석 작업을 자동화하여 개발 효율성을 극대화합니다.

***

### 2. SageMaker JumpStart란?

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

[Amazon SageMaker JumpStart](https://docs.aws.amazon.com/ko_kr/sagemaker/latest/dg/studio-jumpstart.html)는 AI 모델 도입과 구축을 가속화하는 포괄적인 모델 허브입니다. 복잡한 인프라 구축이나 모델 튜닝 과정을 처음부터 수행할 필요 없이, 사전 훈련된(Pre-trained) 수백 개의 인기 있는 오픈 소스 모델과 독점 모델들을 클릭 몇 번만으로 자신의 AWS 계정에 배포할 수 있습니다.

* **사전 훈련된 모델 제공**: 사전 훈련된(Pre-trained) 수백 개의 인기 있는 오픈 소스 모델과 독점 모델 제공
* **유연한 튜닝 및 학습**: 일부 모델은 사용자의 데이터에 맞춰 Fine-tuning 할 수 있도록 옵션 제공
* **통합 관리 환경**: 모델 배포, Fine-tuning, 성능 평가까지의 전 과정을 한곳에서 간편하게 수행

#### 💡 왜 Cosmos Reason 모델을 SageMaker JumpStart로 배포해야 할까요?

**클릭 한 번으로 끝나는 간편한 배포 (One-Click Deployment)**

복잡한 컨테이너 설정이나 GPU 최적화 작업을 직접 할 필요가 없습니다. SageMaker 콘솔이나 Python SDK를 통해 단 몇 번의 클릭(또는 코드 몇 줄)만으로 최신 AI 모델을 배포할 수 있습니다.

**NVIDIA NIM과 통합되어 최적화된 성능**

[NVIDIA NIM](https://www.nvidia.com/ko-kr/ai-data-science/products/nim-microservices/)은 NVIDIA 가속 인프라에 맞춰 추론을 최적화한 마이크로서비스입니다. JumpStart는 이를 완벽하게 지원하여, 모델이 최고의 속도와 효율성으로 실행되도록 보장합니다.

**강력한 보안과 확장성**

AWS의 검증된 인프라 위에서 실행되므로 데이터 보안(VPC 내 배포 등)을 유지할 수 있으며, 트래픽에 따라 유연하게 인프라를 확장할 수 있어 연구 단계부터 실제 서비스 단계까지 매끄럽게 연결됩니다.

***

### 3. Getting Started

#### 3.1 모델 허브에서 `NVIDIA Cosmos Reason` 모델 검색

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### 3.2 모델 Deploy

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

#### 3.3 배포 옵션 설정

모델을 배포할 인스턴스 타입과 개수 설정

***

### References

* [**\[AWS News\]** Build Production-Ready Drug Discovery and Robotics Pipelines with NVIDIA NIMs on SageMaker JumpStart](https://aws.amazon.com/ko/about-aws/whats-new/2026/02/accelerate-biosciences-and-robotics-with-NVIDIA-NIMs-on-sagemaker-jumpstart/)
* [**\[NVIDIA Research\]** Cosmos-Reason1](https://research.nvidia.com/labs/dir/cosmos-reason1/)
* [**\[Github\]** nvidia-cosmos/cosmos-reason1](https://github.com/nvidia-cosmos/cosmos-reason1)
* [**\[HuggingFace\]** nvidia/Cosmos-Reason1-7B](https://huggingface.co/nvidia/Cosmos-Reason1-7B)
