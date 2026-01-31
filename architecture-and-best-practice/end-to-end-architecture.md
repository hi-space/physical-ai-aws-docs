---
description: Physical AI 전체 파이프라인을 AWS에서 닫는 기준 아키텍처
---

# End-to-end Architecture

이 문서는 Physical AI 시스템을 AWS에서 구성할 때의 **기준선**입니다. 세부 구현은 각 영역(Data/Training/Simulation/Sim2Real/Orchestration)에서 다룹니다.

### 논리 아키텍처

* Edge(현장)
  * 센서 수집, 액츄에이터, 실시간 추론
* Cloud(학습/운영)
  * 데이터 레이크, 학습/평가 파이프라인, 시뮬레이션, 모델 레지스트리
* Control plane
  * 오케스트레이션, 승인/릴리즈, 관측, 감사

