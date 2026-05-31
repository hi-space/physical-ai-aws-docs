---
description: AWS 리소스 관리 및 SSH 접속 실무 가이드
---

# 부록. Quick Wrap-up

## DCV 인스턴스 재시작 후 변경된 Public DNS 업데이트

비용 절감을 위해 DCV EC2 인스턴스를 **Stop → Start** 하면 EBS 볼륨/탄력적 IP가 따로 없는 한 **Public DNS가 바뀝니다**. CloudFront distribution의 origin이 이전 DNS를 가리키고 있어 사용자에게 502 Bad Gateway가 노출됩니다.

CDK는 모든 리소스에 `UserId=<userId>` 태그를 자동으로 부여하므로, 그 태그를 키로 EC2와 CloudFront를 매칭해 origin DomainName만 교체하는 작은 스크립트로 해결할 수 있습니다.

### 동작 흐름

1. `tag:UserId=<userId>` + `state:running` 조건으로 EC2의 현재 `PublicDnsName` 조회
2. `tag:UserId=<userId>` 인 CloudFront distribution 찾기
3. `get-distribution-config` → origin `DomainName`만 새 DNS로 치환 → `update-distribution` 호출 (ETag 매칭 필요)
4. 이미 동일하면 no-op

### 사용 예

```bash
# DCV 인스턴스 stop → start 직후
cd ~/aws-physical-ai-recipes/e2e-workshop/infra/isaaclab
./scripts/fix-cloudfront-origin.sh <userId>
```

```bash
# 같은 userId로 Stable/Latest 두 스택을 모두 배포한 경우 (구분자 추가)
./scripts/fix-cloudfront-origin.sh <userId> stable
./scripts/fix-cloudfront-origin.sh <userId> latest
```

### 결과

해당 스크립트 실행 후 아래와 같은 결과가 나옵니다. 아래 결과를 복사해놓고 사용하세요.

```bash
=== 결과 ===
Instance ID:      i-0ecd614dfd1f00b67
Public IP:        54.91.222.154
Public DNS:       ec2-54-91-222-154.compute-1.amazonaws.com
Distribution ID:  E1VCKFYZE31CU6
DCV URL:          https://54.91.222.154:8443
code-server URL:  https://d35g3id1b3geim.cloudfront.net
```

### 전제 조건

- CDK가 이미 모든 리소스에 `UserId` 태그를 자동 적용 중 (`isaac-lab-stack.ts`의 `cdk.Tags.of(this).add('UserId', userId)`) — CloudFront에도 동일 태그가 전파됨
- 대상 EC2 인스턴스가 `running` 상태여야 `PublicDnsName`이 존재

{% hint style="info" %}
CloudFront update가 완료된 뒤에도 엣지 로케이션 전파에 보통 **2~5분**이 걸립니다. 그 사이 502가 잠시 더 보일 수 있으니 캐시 새로고침 전 잠깐 기다려 주세요.
{% endhint %}

---

## CloudShell `npm install` 시 `no space left on device` 해결

CloudShell은 홈 디렉토리(`/home/cloudshell-user`)에 약 **1GB**의 영구 스토리지만 제공합니다. CDK 배포를 반복하거나 여러 워크숍 모듈을 번갈아 사용하면서 `node_modules`가 누적되면 새 `npm install` 시 다음과 같은 에러가 발생할 수 있습니다.

```
npm ERR! code ENOSPC
npm ERR! syscall write
npm ERR! errno -28
npm ERR! nospc ENOSPC: no space left on device
```

먼저 어디가 차 있는지 확인합니다:

```bash
df -h ~                                                     # 홈 사용량/한도
du -sh ~/.npm ~/.cache ~/.cdk ~/aws-physical-ai-recipes 2>/dev/null
du -ah ~ 2>/dev/null | sort -rh | head -20                  # 큰 파일/폴더 Top 20
```

### 해결 방법 1: npm 캐시와 불필요한 node_modules 제거

```bash
# npm 캐시 비우기
npm cache clean --force
rm -rf ~/.npm

# 다른 모듈에서 만든 node_modules 정리 (현재 작업 중인 디렉토리는 제외)
find ~/aws-physical-ai-recipes -type d -name node_modules -prune -print
# 위 출력에서 사용하지 않는 경로만 골라 삭제
rm -rf <위에서 확인한 경로>
```

### 해결 방법 2: CDK 산출물·기타 캐시 정리

CDK는 합성할 때마다 `cdk.out`에 수백 MB의 asset(zip, Docker context)을 쌓아둡니다. 그 외 pip·yarn·pnpm·HuggingFace 캐시도 의외로 큽니다.

```bash
# CDK 산출물 (다음 cdk deploy/synth 시 다시 생성됨)
find ~/aws-physical-ai-recipes -type d -name cdk.out -prune -print
rm -rf <위에서 확인한 경로>
rm -rf ~/.cdk                                # CDK CLI 자체 캐시

# 기타 패키지 매니저 캐시
rm -rf ~/.cache/pip ~/.cache/yarn ~/.cache/pnpm
rm -rf ~/.cache/huggingface ~/.cache/torch    # 모델/데이터 캐시 (있다면)
```

### 해결 방법 3: 작업 디렉토리 자체를 `/tmp`에 두기

CDK 프로젝트 전체를 `/tmp`로 옮기면 `node_modules`와 `cdk.out`까지 임시 영역에 들어가 홈 용량에 영향이 없습니다.

```bash
cp -r ~/aws-physical-ai-recipes/e2e-workshop/infra/groot /tmp/
cd /tmp/groot
npm install
npx cdk deploy ...
```

> 세션이 종료되면 `/tmp`가 사라지므로 `cdk.context.json` 같은 캐시 파일은 별도로 백업해두거나, 배포가 끝난 직후 `cp` 로 홈에 복사해 두세요.

### 해결 방법 4: CloudShell 홈 디렉토리 리셋 (최후의 수단)

위 방법으로도 공간이 안 나오거나 정체불명의 잔여 파일이 많을 때는 CloudShell 환경 자체를 초기화할 수 있습니다.

CloudShell 우상단 **Actions → Delete AWS CloudShell home directory** 선택. 단, **홈 디렉토리에 저장한 모든 파일이 삭제**되므로 보존이 필요한 자료는 미리 S3나 로컬로 옮긴 뒤 진행하세요.

---
