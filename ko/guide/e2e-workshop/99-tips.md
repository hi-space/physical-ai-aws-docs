---
description: AWS 리소스 관리 및 SSH 접속 실무 가이드
---

# 부록. 실무 팁 및 참고 사항

## 1. Amazon S3 스토리지 관리

[Amazon S3(Simple Storage Service)](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/Welcome.html)는 학습 데이터, 모델 체크포인트, 결과물을 저장하는 객체 스토리지입니다. 용량 제한 없이 데이터를 저장하고 어디서든 접근할 수 있어, 학습 파이프라인의 입출력 저장소로 활용합니다.

{% hint style="info" %}
S3 CLI 명령어를 사용하려면 AWS CLI가 설치되어 있어야 합니다. 설치 방법은 [AWS CLI 설치 가이드](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)를 참고하세요.
{% endhint %}

### 1.1 S3 버킷 생성

```bash
# 버킷 생성 (버킷 이름은 전세계에서 유일해야 함)
aws s3 mb s3://<BucketName> --region <Region>

# 예시
aws s3 mb s3://isaaclab-workshop-user01 --region us-east-1
```

{% hint style="warning" %}
S3 버킷 이름은 **전세계적으로 고유**해야 합니다. 소문자, 숫자, 하이픈(-)만 사용 가능하며 3~63자여야 합니다. 팀명이나 사용자명을 포함하여 고유하게 만드세요.
{% endhint %}

### 1.2 권장 폴더 구조 (예시)

로봇 강화학습 프로젝트에서는 아래와 같은 폴더 구조를 권장합니다. CLI 명령어에서 S3 경로를 지정할 때 `s3://<BucketName>/` 뒤에 아래 폴더 경로를 붙여 사용합니다. (예: `s3://isaaclab-workshop-user01/checkpoints/pretrained/agent_72000.pt`)

```
s3://<BucketName>/
├── datasets/                    # 학습 데이터셋
│   ├── demonstrations/          # VLA 학습 데이터
│   └── episodes/                # 에피소드 녹화 데이터
├── assets/                      # IsaacSim 에셋
│   ├── robots/                  # 로봇 URDF/USD 파일
│   └── environments/            # 환경 USD 씬, 오브젝트 등
├── checkpoints/                 # 모델 체크포인트
│   ├── pretrained/              # 사전 학습 모델 (agent_72000.pt 등)
│   └── experiments/             # 실험별 학습 결과
│       ├── h1-rough-v1/         # 실험 이름으로 구분
│       │   ├── best_agent.pt
│       │   └── config.yaml
│       └── h1-rough-v2/
├── logs/                        # 학습 로그
│   ├── tensorboard/             # TensorBoard 이벤트 파일
│   └── cloudwatch/              # 내보낸 CloudWatch 로그
└── results/                     # 최종 결과물
    ├── videos/                  # 시뮬레이션 녹화 영상
    └── reports/                 # 평가 리포트
```

### 1.3 주요 CLI 명령어

로컬에서 학습한 모델이나 데이터를 클라우드(S3)에 업로드하거나, 클라우드에 저장된 체크포인트·에셋 파일을 로컬로 다운로드할 때 아래 명령어를 사용합니다.

**버킷 목록 확인**

생성된 S3 버킷 목록을 조회합니다.

```bash
aws s3 ls
```

**버킷 내 파일 목록 확인**

특정 경로 아래의 파일과 폴더를 조회합니다. 경로 끝에 `/`를 붙여야 해당 폴더 내부를 볼 수 있습니다.

```bash
aws s3 ls s3://<BucketName>/checkpoints/
```

**파일 업로드 (로컬 → S3)**

`cp` 뒤에 `<source> <target>` 순서로 지정합니다. 폴더 전체를 올리려면 `--recursive`를 붙입니다.

```bash
# 단일 파일
aws s3 cp ./best_agent.pt s3://<BucketName>/checkpoints/best_agent.pt

# 폴더 전체
aws s3 cp ./results/ s3://<BucketName>/results/ \
  --recursive
```

**파일 다운로드 (S3 → 로컬)**

source와 target 방향을 반대로 하면 S3에서 로컬로 다운로드합니다. 클라우드에 저장된 체크포인트를 가져올 때 사용합니다.

```bash
# 단일 파일
aws s3 cp s3://<BucketName>/checkpoints/agent_72000.pt ./agent_72000.pt

# 폴더 전체
aws s3 cp s3://<BucketName>/checkpoints/ ./checkpoints/ \
  --recursive
```

**디렉토리 동기화 (변경된 파일만 전송)**

`sync`는 source와 target을 비교하여 변경된 파일만 전송합니다. 학습 로그처럼 계속 쌓이는 데이터를 주기적으로 백업할 때 유용합니다.

```bash
aws s3 sync ./logs/ s3://<BucketName>/logs/
```

**파일 삭제 (실패한 실험 체크포인트 정리 등)**

`rm`으로 S3 객체를 삭제합니다. `--recursive`를 붙이면 해당 경로 하위 전체를 삭제합니다.

```bash
aws s3 rm s3://<BucketName>/checkpoints/failed-run/ \
  --recursive
```

**임시 공유 URL 생성 (1시간 유효)**

`presign`은 인증 없이 접근 가능한 임시 URL을 생성합니다. `--expires-in`으로 유효 시간(초)을 지정합니다. 팀원에게 파일을 빠르게 공유할 때 유용합니다.

```bash
aws s3 presign s3://<BucketName>/checkpoints/best_agent.pt \
  --expires-in 3600
```

{% hint style="info" %}
대용량 체크포인트 파일 전송 시 `aws s3 sync`를 사용하면 변경된 파일만 전송하므로 시간과 비용을 절약할 수 있습니다.
{% endhint %}

<details>
<summary><strong>S3 접근 시 AccessDenied 에러가 발생하는 경우</strong></summary>

워크숍 인프라를 CDK로 배포한 경우 S3 권한이 이미 포함되어 있습니다. 그 외 환경에서 `AccessDenied` 에러가 발생하면 아래를 실행하세요.

```bash
# 현재 인스턴스의 IAM Role 확인
aws sts get-caller-identity

# S3 Full Access 권한 추가
ROLE_NAME=$(aws sts get-caller-identity --query Arn --output text | grep -oP 'assumed-role/\K[^/]+')
aws iam attach-role-policy \
  --role-name "$ROLE_NAME" \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

</details>

***

## 2. Elastic File System (EFS) 활용

[Amazon EFS](https://docs.aws.amazon.com/ko_kr/efs/latest/ug/whatisefs.html)는 여러 인스턴스와 컨테이너에서 동시에 접근 가능한 공유 파일시스템입니다. EC2 인스턴스와 AWS Batch 컨테이너가 동일한 EFS를 마운트하여, 학습 결과를 별도 복사 없이 바로 공유할 수 있습니다.
EFS는 용량 제한이 없으며 사용량에 따라 자동 확장됩니다. 별도의 용량 프로비저닝이 필요 없습니다.

**이 워크숍에서 EFS를 사용하는 이유:**
* AWS Batch 작업은 일시적(ephemeral)입니다. 작업 완료 후 컨테이너가 종료되면 로컬 스토리지의 데이터가 사라집니다
* EFS에 체크포인트를 저장하면 Batch 작업 종료 후에도 학습 결과가 보존됩니다
* 동일한 EFS를 EC2 DCV 인스턴스에 마운트하여 학습 결과를 바로 추론에 활용할 수 있습니다
* 멀티노드 학습 시 메인 노드가 EFS에 체크포인트를 쓰면 다른 노드도 동일한 파일에 접근 가능합니다

### 2.1 EFS 마운트

EFS 콘솔에서 파일 시스템 ID를 확인한 후 마운트합니다.

```bash
# EFS 마운트 (리전을 배포 환경에 맞게 수정)
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport \
  <FileSystemID>.efs.<Region>.amazonaws.com:/ /home/ubuntu/environment/efs
```

```bash
# 마운트 확인
df -h | grep efs
```

{% hint style="warning" %}
마운트 명령은 Docker 컨테이너가 아닌 **호스트(EC2 인스턴스)**에서 실행해야 합니다.
{% endhint %}

### 2.2 Docker 컨테이너에서 EFS 사용

Docker 실행 시 `-v` 옵션으로 호스트의 EFS 디렉토리를 컨테이너 내부에 마운트합니다.

```bash
docker run --gpus all -it \
    -v /home/ubuntu/environment/efs:/workspace/IsaacLab/TrainedModel \
    isaaclab-batch:latest
```

| 호스트 경로 | 컨테이너 경로 | 용도 |
|------------|-------------|------|
| `/home/ubuntu/environment/efs` | `/workspace/IsaacLab/TrainedModel` | 학습 모델 공유 |
| `/home/ubuntu/environment/efs/models` | `/efs/models` (Batch) | Batch 학습 결과 저장 |

***

## 3. Elastic Container Registry (ECR) 관리

[Amazon ECR](https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/what-is-ecr.html)은 Docker 컨테이너 이미지를 저장하는 중앙 레지스트리입니다. Isaac Sim + Isaac Lab 환경이 포함된 이미지를 ECR에 푸시하면, AWS Batch의 여러 노드에서 동일한 이미지를 풀하여 일관된 환경에서 학습을 실행합니다.

### 3.1 ECR 로그인

```bash
aws ecr get-login-password --region <Region> | docker login --username AWS --password-stdin <AccountID>.dkr.ecr.<Region>.amazonaws.com
```

### 3.2 이미지 빌드 및 푸시

```bash
# Docker 이미지 빌드
docker build -t isaaclab-batch .

# ECR 레포지토리에 맞게 태그
docker tag isaaclab-batch:latest <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest

# ECR에 푸시
docker push <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest
```

### 3.3 이미지 풀

```bash
docker pull <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest
```

### 3.4 이미지 관리 팁

* **태그 전략**: `latest` 대신 버전 태그 사용 권장 (예: `isaaclab-batch:v1.0`, `isaaclab-batch:experiment-001`)
* **미사용 이미지 정리**: ECR 콘솔에서 Lifecycle Policy를 설정하여 오래된 이미지를 자동 삭제
* **이미지 스캔**: ECR의 이미지 스캔 기능으로 보안 취약점 확인 가능

***

## 4. IsaacLab EC2 인스턴스 SSH 접속 가이드

DCV 원격 데스크톱의 브라우저 터미널은 입력 지연이 크고 복사/붙여넣기가 불편합니다. SSH로 직접 접속하면 로컬 터미널과 동일한 속도로 작업할 수 있으며, VS Code Remote-SSH 확장을 연결하면 파일 탐색·편집·디버깅까지 로컬 개발 환경과 같은 경험을 얻을 수 있습니다.

### 4.1 자동화 스크립트로 간편 설정

아래 스크립트를 사용하면 SSH 키 생성부터 config 설정까지 한 번에 완료할 수 있습니다. `<PUBLIC_IP>`를 전달받은 EC2 인스턴스 IP로 교체하세요.

{% tabs %}
{% tab title="Mac / Linux" %}
```bash
# [로컬]
curl -fsSL https://raw.githubusercontent.com/hi-space/aws-physical-ai-recipes/main/tools/01-setup-ssh-client.sh -o setup-ssh.sh
bash setup-ssh.sh <PUBLIC_IP>
```
{% endtab %}

{% tab title="Windows (PowerShell)" %}
```powershell
# [로컬] PowerShell을 관리자 권한으로 실행
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/hi-space/aws-physical-ai-recipes/main/tools/01-setup-ssh-client.ps1" -OutFile "setup-ssh.ps1"
.\setup-ssh.ps1 <PUBLIC_IP>
```
{% endtab %}
{% endtabs %}

스크립트 실행 후 안내에 따라 EC2 인스턴스에 공개키를 등록하면 바로 접속 가능합니다.

### 4.2 수동 설정 (단계별)

아래는 각 단계를 수동으로 진행하고 싶은 경우의 가이드입니다.

#### 4.2.1 SSH 키 생성 (키가 없는 경우)

```bash
# [로컬]
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
```

#### 4.2.2 SSH Config 설정

로컬 PC의 `~/.ssh/config` 파일에 아래 내용을 추가합니다. `<PUBLIC_IP>`를 전달받은 IP로 교체하세요.

```
# [로컬] ~/.ssh/config 에 추가
Host isaaclab
    HostName <PUBLIC_IP>
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519
```

#### 4.2.3 최초 접속 (공개키 등록)

**Step 1.** 로컬 공개키 내용을 확인합니다.

```bash
# [로컬]
cat ~/.ssh/id_ed25519.pub
```

**Step 2.** AWS 콘솔에서 EC2 Instance Connect로 인스턴스에 접속한 뒤, 출력된 공개키를 등록합니다.

1. AWS 콘솔 > EC2 > 인스턴스 선택 > **연결** > **EC2 Instance Connect** 탭 > **연결**
2. 브라우저 터미널이 열리면 아래 명령 실행:

```bash
# [EC2 인스턴스] — 브라우저 터미널에서 실행
echo "<PUBLIC_KEY_내용>" >> /home/ubuntu/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
```

#### 4.2.4 이후 접속

공개키가 등록된 후에는 아래 명령으로 바로 접속할 수 있습니다:

```bash
# [로컬]
ssh isaaclab
```

SSH 접속이 완료되면 DCV 원격 데스크톱 대신 터미널 기반으로 모든 작업을 수행할 수 있습니다.

### 4.3 VS Code Remote-SSH로 접속

VS Code의 Remote-SSH 확장을 사용하면 원격 EC2 인스턴스의 파일을 로컬에서 편집하듯 작업할 수 있습니다.

#### 확장 설치

1. VS Code 좌측 **Extensions** 패널(또는 `Ctrl+Shift+X`) 열기
2. `Remote - SSH` 검색 → **Microsoft** 게시자의 확장 설치

#### 접속 방법

1. `Ctrl+Shift+P` (Mac: `Cmd+Shift+P`) → **Remote-SSH: Connect to Host...** 선택
2. 목록에서 `isaaclab` 선택 (4.1 또는 4.2에서 설정한 SSH config의 Host 이름)
3. 새 VS Code 창이 열리며 원격 인스턴스에 연결

#### 원격 폴더 열기

접속 후 **File > Open Folder...** 에서 작업 디렉토리를 선택합니다.

```
/home/ubuntu/workspace/IsaacLab
```

이제 파일 탐색, 편집, 터미널, 디버깅 등 모든 VS Code 기능을 로컬과 동일하게 사용할 수 있습니다.
