---
description: AWS 기반 Physical AI 시스템 구축 및 운영 가이드
icon: user-robot
metaLinks:
  alternates:
    - https://app.gitbook.com/s/eiiMMZMGOJGOQZP7MGcd/introduction/readme
---

# Physical AI on AWS

{% hint style="danger" %}
본 Gitbook은 저자의 개인적인 의견을 바탕으로 작성된 가이드로, 재직 중인 회사의 입장 및 공식문서를 대변하지 않습니다.
{% endhint %}

### 이 가이드에 대하여

최근 Physical AI가 주목받고 있지만, 여러 기술의 집합체이다 보니 전체 기술 스택을 한눈에 파악하기 쉽지 않습니다. Physical AI는 단순히 하나의 AI 모델이 아니라, **AI가 물리적 세계와 상호작용하며 작동하는 통합 시스템**을 의미합니다.

이 가이드는 Physical AI의 핵심 기술 요소를 체계적으로 이해하고, AWS 클라우드에서 실용적으로 구현하는 방법을 전달하기 위해 작성되었습니다. 데이터 수집부터 학습, 시뮬레이션, Sim2Real, 배포, 에이전틱 오케스트레이션까지 전체 파이프라인을 다룹니다.

최근 Physical AI가 주목받고 있지만, 여러 기술의 집합체이다 보니 전체 기술 스택을 한눈에 파악하기 쉽지 않습니다. Physical AI는 단순히 하나의 AI 모델이 아니라, **AI가 물리적 세계와 상호작용하며 작동하는 통합 시스템**을 의미합니다.

이 가이드는 Physical AI의 핵심 기술 요소를 체계적으로 이해하고, AWS 클라우드에서 실용적으로 구현하는 방법을 전달하기 위해 작성되었습니다. 데이터 수집부터 학습, 시뮬레이션, Sim2Real, 배포, 에이전틱 오케스트레이션까지 전체 파이프라인을 다룹니다.

***

### 대상 독자

* Physical AI 워크로드를 담당하는 **클라우드/인프라 엔지니어**
* 로봇, 자율주행, 드론 팀의 **AI/플랫폼 엔지니어**
* Physical AI 기술에 관심 있는 **Tech Lead / PM**

***

### 작성자 소개

자율주행차량 및 로보틱스 AI 연구개발 경험을 바탕으로 AWS 아키텍처 설계와 기술 지원을 담당하고 있습니다. 현재 Agentic AI와 Physical AI에 집중하고 있습니다.

* **LinkedIn**: [https://www.linkedin.com/in/yoo-lee](https://www.linkedin.com/in/yoo-lee/)
* **GitHub**: [https://github.com/hi-space](https://github.com/hi-space)
