---
description: >-
  RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control
  (2023)
---

# DeepMind RT-2

> _본 연구는 인터넷 규모의 데이터로 학습된 비전-언어 모델(Vision-Language Model, VLM)을 엔드-투-엔드(end-to-end) 로봇 제어에 직접 통합하여 일반화 능력을 향상시키고 창발적 의미 추론(emergent semantic reasoning)을 가능하게 하는 방법을 다룹니다._

### 들어가며

광범위한 웹 규모 데이터셋으로 사전 학습된 고용량 모델은 다양한 하위 작업(downstream task)을 위한 효과적이고 강력한 플랫폼을 제공합니다. 대규모 언어 모델(Large Language Model, LLM)은 유창한 텍스트 생성뿐만 아니라 창발적 문제 해결, 산문 및 코드의 창의적 생성을 가능하게 하며, 비전-언어 모델(Vision-Language Model, VLM)은 개방형 어휘 시각 인식(open-vocabulary visual recognition)을 가능하게 하고 이미지에서 객체-에이전트 상호작용에 대한 복잡한 추론까지 수행할 수 있습니다. 이러한 의미론적 추론(semantic reasoning), 문제 해결, 시각적 해석 능력은 실제 환경에서 다양한 작업을 수행해야 하는 범용 로봇(generalist robot)에게 매우 유용할 것입니다. 그러나 로봇이 이러한 능력을 어떻게 습득해야 하는지는 명확하지 않습니다. 무차별 대입 방식(brute force approach)으로는 수백만 건의 로봇 상호작용 시험을 수집해야 할 수 있지만, 가장 유능한 언어 및 비전-언어 모델은 웹에서 수십억 개의 토큰과 이미지로 학습됩니다. 이는 가까운 미래에 로봇 데이터로 따라잡기 어려운 양입니다.

***

### 배경: 기존 접근 방식의 한계

**비전-언어 모델(Vision-Language Model, VLM)**. 비전-언어 모델에는 여러 범주가 있으며, 가장 관련성이 높은 두 가지는 다음과 같습니다.

(1) CLIP과 같은 표현 학습 모델(representation-learning model)로, 두 모달리티(modality)에 대한 공통 임베딩(embedding)을 학습합니다.

(2) {vision, text} → {text} 형태의 시각 언어 모델로, 시각과 언어를 입력으로 받아 자유 형식 텍스트를 제공합니다.

두 범주 모두 객체 분류(object classification), 탐지(detection), 분할(segmentation)과 같은 다양한 하위 애플리케이션에 사전 학습을 제공하는 데 사용되었습니다. 이러한 모델은 일반적으로 이미지 캡셔닝(image captioning), 시각적 질의응답(VQA), 여러 데이터셋에 대한 일반 언어 작업과 같은 다양한 작업으로 동시에 학습됩니다. 선행 연구들이 로봇 공학을 포함한 광범위한 문제와 설정에서 VLM을 연구했지만, 우리의 초점은 VLM에 로봇 행동을 예측하는 능력을 부여하여 VLM의 능력을 로봇 폐쇄 루프 제어(closed-loop control)로 확장하는 방법에 있습니다. 이를 통해 VLM에 이미 존재하는 지식을 활용하여 새로운 수준의 일반화를 가능하게 합니다.

***

### 핵심 아이디어

### 주요 기여

1. 우리의 목표는 로봇의 관찰을 행동으로 매핑하는 것을 학습하면서 동시에 웹의 언어 및 비전-언어 데이터에 대한 대규모 사전 학습(pretraining)의 이점을 누릴 수 있는 단일 엔드-투-엔드 모델을 구현하는 것입니다
2. 이를 위해 우리는 최신 비전-언어 모델을 로봇 궤적 데이터와 시각적 질의응답(Visual Question Answering, VQA)과 같은 인터넷 규모의 비전-언어 작업에 함께 파인튜닝(co-fine-tune)할 것을 제안합니다
3. 다른 접근법과 달리 다른 접근법과 달리, 우리는 간단하고 일반적인 방법을 제안합니다
4. 자연어 응답과 로봇 행동을 동일한 형식에 맞추기 위해 행동을 텍스트 토큰으로 표현하고 자연어 응답과 로봇 행동을 동일한 형식에 맞추기 위해 행동을 텍스트 토큰으로 표현하고 자연어 토큰과 동일한 방식으로 모델의 학습 데이터에 직접 통합합니다
5. 우리는 이러한 범주의 모델을 비전-언어-행동 모델(Vision-Language-Action, VLA)이라 부르며, 그 예시로 RT-2라는 모델을 구현했습니다

***

### 실험 결과

우리의 실험은 RT-2의 실제 환경 일반화(real-world generalization)와 창발적 능력(emergent capability)에 초점을 맞추며, 다음 질문에 답하고자 합니다.

1\) RT-2는 학습된 작업에서 어떻게 수행되며, 더 중요하게는 새로운 객체, 배경, 환경에 대해 어떻게 일반화되는가?

2\) RT-2의 창발적 능력을 관찰하고 측정할 수 있는가?

3\) 일반화 능력이 매개변수 수와 다른 설계 결정에 따라 어떻게 달라지는가?

4\) RT-2가 비전-언어 모델과 유사하게 사고의 연쇄(chain-of-thought) 추론의 징후를 보일 수 있는가?

RT-2는 학습된 작업에서는 RT-1과 유사한 성능을 보이지만, 새로운 객체, 배경, 환경에 대한 일반화에서 탁월한 성능을 보입니다. 평균적으로 RT-2는 다음 베이스라인인 RT-1과 MOO에 비해 약 2배 높은 성공률을 기록했으며, 다른 베이스라인보다는 약 6배 더 우수한 성능을 보였습니다. 이는 VLM의 인터넷 규모 사전 학습이 시각적 및 의미론적 개념에 대한 더 일반화 가능한 지식을 전이함을 시사합니다.

***

### 실용적 의미

본 논문에서 우리는 비전-언어 모델(VLM) 사전 학습과 로봇 데이터를 결합하여 비전-언어-행동(Vision-Language-Action, VLA) 모델을 학습시키는 방법을 설명했습니다. 그런 다음 PaLM-E와 PaLI-X를 기반으로 한 두 가지 VLA 인스턴스인 RT-2-PaLM-E와 RT-2-PaLI-X를 제시했습니다. 이러한 모델은 로봇 궤적 데이터와 함께 공동 파인튜닝되어 텍스트 토큰으로 표현되는 로봇 행동을 출력합니다. 우리는 우리의 접근법이 매우 고성능의 로봇 정책으로 이어지며, 더 중요하게는 웹 규모 비전-언어 사전 학습으로부터 상속된 훨씬 더 나은 일반화 성능과 창발적 능력으로 이어진다는 것을 보여주었습니다. 우리는 이 간단하고 일반적인 접근법이 로봇 공학이 더 나은 비전-언어 모델로부터 직접 혜택을 받을 수 있는 가능성을 보여준다고 믿으며, 이는 로봇 학습 분야를 다른 분야의 발전과 함께 더욱 개선할 수 있는 전략적 위치에 놓습니다.

***

### 마치며

본 논문에서 우리는 비전-언어 모델(VLM) 사전 학습과 로봇 데이터를 결합하여 비전-언어-행동(Vision-Language-Action, VLA) 모델을 학습시키는 방법을 설명했습니다. 그런 다음 PaLM-E와 PaLI-X를 기반으로 한 두 가지 VLA 인스턴스인 RT-2-PaLM-E와 RT-2-PaLI-X를 제시했습니다. 이러한 모델은 로봇 궤적 데이터와 함께 공동 파인튜닝되어 텍스트 토큰으로 표현되는 로봇 행동을 출력합니다. 우리는 우리의 접근법이 매우 고성능의 로봇 정책으로 이어지며, 더 중요하게는 웹 규모 비전-언어 사전 학습으로부터 상속된 훨씬 더 나은 일반화 성능과 창발적 능력으로 이어진다는 것을 보여주었습니다. 우리는 이 간단하고 일반적인 접근법이 로봇 공학이 더 나은 비전-언어 모델로부터 직접 혜택을 받을 수 있는 가능성을 보여준다고 믿으며, 이는 로봇 학습 분야를 다른 분야의 발전과 함께 더욱 개선할 수 있는 전략적 위치에 놓습니다.

***

### **핵심 요약**

* 본 논문에서 우리는 비전-언어 모델(VLM) 사전 학습과 로봇 데이터를 결합하여 비전-언어-행동(Vision-Language-Action, VLA) 모델을 학습시키는 방법을 설명했습니다
* 그런 다음 PaLM-E와 PaLI-X를 기반으로 한 두 가지 VLA 인스턴스인 RT-2-PaLM-E와 RT-2-PaLI-X를 제시했습니다
* 이러한 모델은 로봇 궤적 데이터와 함께 공동 파인튜닝되어 텍스트 토큰으로 표현되는 로봇 행동을 출력합니다

***

### Citation

```bibtex
@misc{brohan2023rt2visionlanguageactionmodelstransfer,
      title={RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control}, 
      author={Anthony Brohan and Noah Brown and Justice Carbajal and Yevgen Chebotar and Xi Chen and Krzysztof Choromanski and Tianli Ding and Danny Driess and Avinava Dubey and Chelsea Finn and Pete Florence and Chuyuan Fu and Montse Gonzalez Arenas and Keerthana Gopalakrishnan and Kehang Han and Karol Hausman and Alexander Herzog and Jasmine Hsu and Brian Ichter and Alex Irpan and Nikhil Joshi and Ryan Julian and Dmitry Kalashnikov and Yuheng Kuang and Isabel Leal and Lisa Lee and Tsang-Wei Edward Lee and Sergey Levine and Yao Lu and Henryk Michalewski and Igor Mordatch and Karl Pertsch and Kanishka Rao and Krista Reymann and Michael Ryoo and Grecia Salazar and Pannag Sanketi and Pierre Sermanet and Jaspiar Singh and Anikait Singh and Radu Soricut and Huong Tran and Vincent Vanhoucke and Quan Vuong and Ayzaan Wahid and Stefan Welker and Paul Wohlhart and Jialin Wu and Fei Xia and Ted Xiao and Peng Xu and Sichun Xu and Tianhe Yu and Brianna Zitkovich},
      year={2023},
      eprint={2307.15818},
      archivePrefix={arXiv},
      primaryClass={cs.RO},
      url={https://arxiv.org/abs/2307.15818}, 
}
```

### 참고 자료

* [**\[Paper\]** RT-2: Vision-Language-Action Models Transfer Web Knowledge to Robotic Control](https://arxiv.org/abs/2307.15818)
* [**\[DeepMind Blog\]** RT-2: New model translates vision and language into action](https://deepmind.google/blog/rt-2-new-model-translates-vision-and-language-into-action/)
