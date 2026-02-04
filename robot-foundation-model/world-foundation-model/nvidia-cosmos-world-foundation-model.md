---
description: World Simulation with Video Foundation Models for Physical AI (2025)
---

# Cosmos-Predict2.5

> **Physical AI를 위한 최신 Cosmos World Foundation Model, Cosmos-Predict 2.5**

### 배경

Physical AI 시스템(로봇, 자율주행 등)은 현실 세계에서 직접 학습하기에는 비용이 높고 위험합니다. 이 논문은\
고품질의 다양한 가상 환경을 생성할 수 있는 세계 시뮬레이터(World simulator)를 제안합니다.

이러한 시뮬레이터를 통해 에이전트는 실제 세계에 배포되기 전에 가상 환경에서 인식 및 제어 기술을 완전히 습득할 수 있습니다. 본 논문에서는 다양한 Physical AI 도메인에서 시뮬레이션 충실도를 크게 향상시킨 플로우 매칭(Flow matching) 기반의 최신 World Foundation Model인 Cosmos-Predict2를 소개합니다.

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

