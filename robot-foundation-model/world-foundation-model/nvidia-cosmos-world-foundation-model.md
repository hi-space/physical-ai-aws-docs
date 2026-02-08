---
description: World Simulation with Video Foundation Models for Physical AI (2025)
---

# Cosmos-Predict2.5

### Overview

NVIDIA의 Cosmos-Predict-2.5는 Flow Matching 기반의 생성 구조를 통해 텍스트, 이미지, 비디오로부터 물리적으로 정확한 World simulation을 생성하는 파운데이션 모델입니다. 2억 개의 큐레이션된 비디오 클립으로 학습되고, 강화학습으로 post-training 됩니다.&#x20;

이 모델은 로봇 공학, 자율주행, 구체화된 지능 등 물리 AI 분야의 실제 응용에서 업계 최고 수준의 성능을 달성합니다. 본 연구는 고급 데이터 처리, 혁신적인 아키텍처, 그리고 다양한 실제 응용 사례를 통해 생성형 비디오 모델이 물리적 세계 이해 및 제어의 새로운 경계를 어떻게 열고 있는지 보여줍니다.



### 배경

#### Physical AI의 필요성

Physical AI 시스템(로봇, 자율주행 등)은 현실 세계에서 직접 학습하기에는 비용이 높고 위험합니다. 이 논문은\
고품질의 다양한 가상 환경을 생성할 수 있는 세계 시뮬레이터(World simulator)를 제안합니다. 이 시뮬레이터를 통해 에이전트는 실제 세계에 배포되기 전에 가상 환경에서 인식 및 제어 기술을 완전히 습득할 수 있습니다.

전통적인 비디오 생성 모델들은 일반적인 콘텐츠 생성에 최적화되어 있어서 로봇이 물건을 집거나 자동차가 도로 위에서 움직이는 것과 같은 물리적으로 일관성 있는 시뮬레이션을 생성하는 데 어려움을 겪고 있습니다. 기존 모델들은 객체의 물리적 상호작용, 정확한 동역학, 그리고 공간-시간적 일관성을 충분히 보장하지 못합니다.

이러한 한계를 극복하기 위해 NVIDIA의 Cosmos 팀은 물리 AI 분야에 특화된 기초 모델을 개발했습니다. 기존의 Cosmos-Predict1과 Cosmos-Transfer1에 비해 Cosmos-Predict-2.5는 더 강력한 성능, 개선된 텍스트 이해, 그리고 더욱 정교한 물리적 추론 능력을 제공합니다.

#### 핵심 도전 과제

물리적으로 일관성 있는 비디오 생성을 위해서는 다음과 같은 도전 과제들을 해결해야 합니다.

1. **고품질 데이터**: 물리 AI에 특화된 충분한 양의 데이터 확보
2. **물리적 일관성**: 시간에 따른 객체의 움직임과 상호작용이 물리 법칙을 준수하도록 보장
3. **제어 가능성**: 텍스트, 이미지, 비디오 등 다양한 입력 양식으로 세계 생성을 제어
4. **효율성**: 실시간 로봇 시스템 등에서 실용적으로 사용 가능한 크기의 모델 제공

이러한 도전 과제들을 해결하기 위해 Cosmos-Predict-2.5는 광범위한 데이터 큐레이션, 혁신적인 아키텍처 설계, 그리고 다단계 학습 전략을 통합합니다.



### 주요 내용

* **통합 아키텍처**: Text2World, Image2World, Video2World를 단일 모델로 통합
* **Flow Matching 기반**: 이전 Cosmos-Predict1의 diffusion 모델에서 업그레이드
* **Cosmos-Reason1 활용**: T5 텍스트 인코더 대신 VLM 기반 아키텍처 사용
* **강화학습 기반 후처리**: 비디오 품질과 프롬프트 정렬도 대폭 향상

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



### **Citation**

```bibtex
@misc{nvidia2025worldsimulationvideofoundation,
      title={World Simulation with Video Foundation Models for Physical AI}, 
      author={NVIDIA and : and Arslan Ali and Junjie Bai and Maciej Bala and Yogesh Balaji and Aaron Blakeman and Tiffany Cai and Jiaxin Cao and Tianshi Cao and Elizabeth Cha and Yu-Wei Chao and Prithvijit Chattopadhyay and Mike Chen and Yongxin Chen and Yu Chen and Shuai Cheng and Yin Cui and Jenna Diamond and Yifan Ding and Jiaojiao Fan and Linxi Fan and Liang Feng and Francesco Ferroni and Sanja Fidler and Xiao Fu and Ruiyuan Gao and Yunhao Ge and Jinwei Gu and Aryaman Gupta and Siddharth Gururani and Imad El Hanafi and Ali Hassani and Zekun Hao and Jacob Huffman and Joel Jang and Pooya Jannaty and Jan Kautz and Grace Lam and Xuan Li and Zhaoshuo Li and Maosheng Liao and Chen-Hsuan Lin and Tsung-Yi Lin and Yen-Chen Lin and Huan Ling and Ming-Yu Liu and Xian Liu and Yifan Lu and Alice Luo and Qianli Ma and Hanzi Mao and Kaichun Mo and Seungjun Nah and Yashraj Narang and Abhijeet Panaskar and Lindsey Pavao and Trung Pham and Morteza Ramezanali and Fitsum Reda and Scott Reed and Xuanchi Ren and Haonan Shao and Yue Shen and Stella Shi and Shuran Song and Bartosz Stefaniak and Shangkun Sun and Shitao Tang and Sameena Tasmeen and Lyne Tchapmi and Wei-Cheng Tseng and Jibin Varghese and Andrew Z. Wang and Hao Wang and Haoxiang Wang and Heng Wang and Ting-Chun Wang and Fangyin Wei and Jiashu Xu and Dinghao Yang and Xiaodong Yang and Haotian Ye and Seonghyeon Ye and Xiaohui Zeng and Jing Zhang and Qinsheng Zhang and Kaiwen Zheng and Andrew Zhu and Yuke Zhu},
      year={2025},
      eprint={2511.00062},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2511.00062}, 
}
```

### References

* [**\[Paper\]** World Simulation with Video Foundation Models for Physical AI](https://arxiv.org/abs/2511.00062)
* [**\[NVIDIA Research\]** Cosmos-Predict2.5](https://research.nvidia.com/labs/dir/cosmos-predict2.5/)
* [**\[Github\]** nvidia-cosmos/cosmos-predict2.5](https://github.com/nvidia-cosmos/cosmos-predict2.5)
* [**\[HuggingFace\]** nvidia/Cosmos-Predict2.5-2B](https://huggingface.co/nvidia/Cosmos-Predict2.5-2B)
* [**\[HuggingFace\]** nvidia/Cosmos-Predict2.5-14B](https://huggingface.co/nvidia/Cosmos-Predict2.5-14B)

