# World Foundation Model

World Foundation Model은 물리적 세계의 디지털 트윈으로, Physical AI가 안전하게 학습하고 실험할 수 있는 가상 환경을 제공하는 AI 모델입니다.

**세계의 동작을 예측**하는 모델로, 과거 관찰 데이터 + 현재의 행동/입력을 바탕으로, 미래 세계 상태를 예측합니다. 로봇 관점에서는 “다음 프레임/다음 상태가 어떻게 될까”를 학습합니다. 정책(Policy)과 결합하면 플래닝과 Sim2Real에 도움됩니다.





### WFM이 쓰이는 곳

* **시뮬레이션 보강**: 현실처럼 보이는 롤아웃 생성.
* **계획(Planning)**: 후보 행동의 결과를 미리 예측.
* **데이터 증강**: 희귀 시나리오를 생성.

### Physical AI와의 관계

```
Physical AI System Architecture
├─ Policy Model (정책)
│  └─ Robot Foundation Model
│     └─ Vision-Language-Action Model
│        ├─ 시각 인식
│        ├─ 언어 이해
│        └─ 액션 생성
│
└─ World Model (환경)
   └─ World Foundation Model
      ├─ 비디오 생성 (Diffusion/Autoregressive)
      ├─ 물리 시뮬레이션
      └─ 상호작용 시나리오
```

* RFM: "로봇이 무엇을 해야 하는가?" → 정책 결정
* WFM: "액션의 결과는 어떻게 되는가?" → 환경 시뮬레이션

#### 기술적 특징

1. **다중 모달리티**: 다양한 시각 정보와 물리 시뮬레이션
2. **3D 일관성**: 시간에 따른 공간적 일관성 유지
3. **정확한 물리**: 실제 물리 법칙을 따르는 예측
4. **확장성**: 대규모 다양한 비디오 데이터로 학습 가능
5. **전이 학습**: 일반 WFM을 특정 도메인으로 효율적 특화

#### 실제 적용

* **데이터 확장 문제 해결**: Physical AI 데이터 수집의 위험성 극복
* **시뮬레이션**: 실제 로봇 배포 전 안전한 정책 학습
* **비용 절감**: 실제 로봇 상호작용 대신 디지털 트윈에서 학습

***

### Cosmos WFM 플랫폼 (NVIDIA)

#### **사전학습 방식**

**1. Diffusion WFM**

* 확산 모델 기반 접근
* 높은 품질의 3D 일관성 있는 비디오 생성
* 정확한 물리 시뮬레이션

**2. Autoregressive WFM**

* 트랜스포머 기반 자동회귀 모델
* 연속 및 이산 잠재 표현 사용
* 비디오의 이산 토큰화를 통한 처리

#### **사후학습 (Post-training) 및 특화**

**대규모 비디오 데이터셋으로 사전학습** → **특정 Physical AI 작업으로 사후학습**

**주요 특화 분야**

1. **카메라 제어**: 시각적 탐색 및 감시
2. **로봇 조작 (Robotic Manipulation)**: 명령 기반 조작 작업
3. **자율주행 (Autonomous Driving)**: 운전 시나리오 시뮬레이션

**데이터 효율성**

* 사전학습된 WFM이 우수한 기초 제공
* 사후학습 데이터셋 크기 대폭 감소 가능
* "프롬프트-비디오" 쌍으로 학습 가능 (액션 명령, 궤적, 지시사항 등)



### References

* [**\[NVIDIA Blog\]** Advancing Physical AI with NVIDIA Cosmos World Foundation Model Platform](https://developer.nvidia.com/blog/advancing-physical-ai-with-nvidia-cosmos-world-foundation-model-platform/)

