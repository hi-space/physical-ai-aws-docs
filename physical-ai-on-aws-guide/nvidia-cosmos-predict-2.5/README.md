# NVIDIA Cosmos Predict 2.5 배포

### 1. 개요

NVIDIA Cosmos Predict는 Physical AI를 위한 World Foundation Model(WFM)로, 텍스트·이미지·비디오 입력으로부터 미래 세계 상태를 비디오 형태로 예측합니다.

이 가이드는 Cosmos Predict 2.5를 AWS에 배포하기 위한 두 가지 레퍼런스 아키텍처와 샘플 코드를 제공합니다.

***

### 2. 모델 스펙 & GPU 요구사항

AWS에서는 보통 아래 선택 기준에 의거하여 두 가지 방식으로 배포할 수 있습니다.

* 지연 시간 요구사항&#x20;
* 추론 트래픽 패턴(상시/피크/간헐)
* 비용 제약
* Physical AI 파이프라인 내 연동 지점(데이터/후처리/학습)

<table data-header-hidden><thead><tr><th width="184">-</th><th>NIM on EKS</th><th>AWS Batch</th><th>SageMaker</th></tr></thead><tbody><tr><td><strong>용도</strong></td><td>실시간 추론 (API 서빙)</td><td>대량 오프라인 배치 처리</td><td>관리형 추론 (실시간/비동기)</td></tr><tr><td><strong>구성</strong></td><td>EKS + NIM + ALB + Karpenter</td><td>Batch + EC2</td><td>SageMaker + ECR + S3</td></tr><tr><td><strong>장점</strong></td><td>저지연, 항시 가용, 자동 스케일링</td><td>Spot으로 60-90% 절감 가능</td><td><strong>Scale-to-0</strong> (비용 0), 완전 관리형</td></tr><tr><td><strong>적합</strong></td><td>인터랙티브 앱, 실시간 서비스</td><td>비실시간 대규모 데이터 생성</td><td>실시간/비동기 추론</td></tr></tbody></table>

#### [**Option 1: 실시간 추론**](cosmos-nim-+-eks.md)

[Amazon EKS](https://aws.amazon.com/eks/)에서 [NVIDIA NIM 마이크로서비스](https://developer.nvidia.com/nim?sortBy=developer_learning_library%2Fsort%2Ffeatured_in.nim%3Adesc%2Ctitle%3Aasc)를 운영합니다. EKS + NIM 패턴은 GPU Pod를 상시 띄우기 때문에, 응답 지연과 가용성을 우선시 하고 지연 시간이 짧습니다. 인터랙티브한 호출에 적합합니다.

#### [Option2: **배치 추론**](aws-batch-+-ec2.md)

[AWS Batch](https://aws.amazon.com/batch/)에서 컨테이너 기반 모델을 잡으로 실행합니다. AWS Batch 패턴은 필요할 때만 컴퓨팅을 띄우기 때문에, 오프라인 워크로드에 적합합니다. 비용 효율과 탄력적 처리량을 우선합니다.

***

### References

{% embed url="https://aws.amazon.com/blogs/hpc/running-nvidia-cosmos-world-foundation-models-on-aws/" %}

{% embed url="https://github.com/nvidia-cosmos/cosmos-predict2.5" %}

{% embed url="https://github.com/NVIDIA/nim-deploy" %}
