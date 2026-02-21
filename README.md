# AWS DevOps MCP 서버 가이드

AI 에디터(VS Code, Cursor, Kiro 등)에서 AWS DevOps 작업을 자동화하는 MCP 서버 설정 가이드입니다.

## 📋 목차
- [사전 요구사항](#사전-요구사항)
- [MCP 서버 구성](#mcp-서버-구성)
- [설치 방법](#설치-방법)
- [MCP 서버 상세 정보](#mcp-서버-상세-정보)
- [사용 예시](#사용-예시)
- [IAM 권한 설정](#iam-권한-설정)
- [문제 해결](#문제-해결)

## 사전 요구사항

### 1. uvx 설치

**macOS:**
```bash
# Homebrew로 설치
brew install uv

# 설치 확인
uvx --version
```

**Linux:**
```bash
# 공식 설치 스크립트
curl -LsSf https://astral.sh/uv/install.sh | sh

# 쉘 재시작
source ~/.zshrc  # 또는 source ~/.bashrc

# 설치 확인
uvx --version
```

### 2. AWS 자격증명 설정

```bash
# AWS CLI 설치 확인
aws --version

# AWS 자격증명 구성
aws configure
```

필요한 정보:
- AWS Access Key ID
- AWS Secret Access Key
- Default region: `us-east-1` (권장)
- Default output format: `json`

## MCP 서버 구성

### 전체 MCP 설정 파일

```json
{
  "mcpServers": {
    "aws-core": {
      "command": "uvx",
      "args": ["awslabs.core-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-ccapi": {
      "command": "uvx",
      "args": ["awslabs.ccapi-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-knowledge": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-cloudwatch": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-iac": {
      "command": "uvx",
      "args": ["awslabs.iac-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-iam": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-cloudtrail": {
      "command": "uvx",
      "args": ["awslabs.cloudtrail-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-prometheus": {
      "command": "uvx",
      "args": ["awslabs.prometheus-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-eks": {
      "command": "uvx",
      "args": ["awslabs.eks-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-ecs": {
      "command": "uvx",
      "args": ["awslabs.ecs-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-serverless": {
      "command": "uvx",
      "args": ["awslabs.serverless-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-lambda-tool": {
      "command": "uvx",
      "args": ["awslabs.lambda-tool-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    }
  }
}
```

## 설치 방법

### VS Code

```bash
# 프로젝트 루트에 설정 폴더 생성
mkdir -p .vscode

# MCP 설정 파일 생성
cat > .vscode/mcp.json << 'EOF'
{
  "mcpServers": {
    "aws-core": {
      "command": "uvx",
      "args": ["awslabs.core-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-ccapi": {
      "command": "uvx",
      "args": ["awslabs.ccapi-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-knowledge": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-cloudwatch": {
      "command": "uvx",
      "args": ["awslabs.cloudwatch-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-iac": {
      "command": "uvx",
      "args": ["awslabs.iac-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-iam": {
      "command": "uvx",
      "args": ["awslabs.iam-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-cloudtrail": {
      "command": "uvx",
      "args": ["awslabs.cloudtrail-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-prometheus": {
      "command": "uvx",
      "args": ["awslabs.prometheus-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-eks": {
      "command": "uvx",
      "args": ["awslabs.eks-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-ecs": {
      "command": "uvx",
      "args": ["awslabs.ecs-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-serverless": {
      "command": "uvx",
      "args": ["awslabs.serverless-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-lambda-tool": {
      "command": "uvx",
      "args": ["awslabs.lambda-tool-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "default",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    }
  }
}
EOF

# VS Code 재시작
```

### Cursor

```bash
# 프로젝트 루트에 설정 폴더 생성
mkdir -p .cursor

# MCP 설정 파일 생성 (위와 동일한 내용)
cp .vscode/mcp.json .cursor/mcp.json

# Cursor 재시작
```

### Kiro

```bash
# 프로젝트 루트에 설정 폴더 생성
mkdir -p .kiro/settings

# MCP 설정 파일 생성 (위와 동일한 내용)
cp .vscode/mcp.json .kiro/settings/mcp.json

# Kiro 재시작
```

## MCP 서버 상세 정보

### 1. AWS Core MCP Server

**역할:** AWS 기본 서비스 관리

**주요 API:**
- EC2, S3, RDS 등 핵심 서비스 관리
- 리소스 조회 및 상태 확인

**사용 예시:**
```
"현재 실행 중인 EC2 인스턴스를 보여줘"
"S3 버킷 목록을 알려줘"
"RDS 인스턴스 상태를 확인해줘"
```

### 2. AWS CCAPI MCP Server

**역할:** AWS Control Center API 통합

**주요 API:**
- 계정 관리
- 조직 구조 관리

**사용 예시:**
```
"AWS 계정 정보를 보여줘"
"조직 구조를 확인해줘"
```

### 3. AWS Knowledge MCP Server

**역할:** AWS 공식 문서 검색 및 참조

**주요 API:**
- `search_documentation`: AWS 문서 검색
- `read_documentation`: 특정 문서 읽기
- `recommend`: 관련 문서 추천

**사용 예시:**
```
"CloudFormation 사용법을 알려줘"
"EKS 베스트 프랙티스를 찾아줘"
"Lambda 문서를 검색해줘"
```

### 4. AWS CloudWatch MCP Server

**역할:** 모니터링 및 로그 분석

**주요 API:**
- 알람 조회 및 분석
- 로그 검색 및 패턴 분석
- 메트릭 조회

**사용 예시:**
```
"현재 발생한 CloudWatch 알람을 보여줘"
"지난 1시간 동안 애플리케이션 로그에서 에러를 찾아줘"
"EC2 CPU 사용률을 확인해줘"
```

### 5. AWS IaC MCP Server

**역할:** 인프라 코드 작성 및 관리

**주요 API:**
- CloudFormation 템플릿 생성
- AWS CDK 코드 생성
- 템플릿 검증

**사용 예시:**
```
"VPC와 EC2를 생성하는 CloudFormation 템플릿을 만들어줘"
"CDK로 Lambda 함수를 배포하는 코드를 작성해줘"
"이 CloudFormation 템플릿을 검증해줘"
```

### 6. AWS IAM MCP Server

**역할:** 보안 및 권한 관리

**주요 API:**
- IAM 사용자, 역할, 그룹 관리
- 정책 생성 및 검증
- 권한 분석

**사용 예시:**
```
"S3 읽기 전용 권한을 가진 IAM 역할을 만들어줘"
"현재 사용자의 권한을 확인해줘"
"이 IAM 정책을 검증해줘"
```

### 7. AWS CloudTrail MCP Server

**역할:** API 활동 추적 및 감사

**주요 API:**
- API 호출 이력 조회
- 보안 이벤트 분석
- 컴플라이언스 감사

**사용 예시:**
```
"최근 24시간 동안 의심스러운 IAM 활동을 찾아줘"
"누가 이 S3 버킷을 삭제했는지 확인해줘"
"특정 사용자의 API 호출 이력을 보여줘"
```

### 8. AWS Prometheus MCP Server

**역할:** Prometheus 메트릭 조회

**주요 API:**
- PromQL 쿼리 실행
- 메트릭 조회 및 분석
- AWS Managed Prometheus 통합

**사용 예시:**
```
"Prometheus에서 CPU 메트릭을 조회해줘"
"PromQL로 메모리 사용률을 확인해줘"
```

### 9. AWS EKS MCP Server

**역할:** Kubernetes 클러스터 관리

**주요 API:**
- 클러스터 조회 및 관리
- 노드 그룹 관리
- 워크로드 배포

**사용 예시:**
```
"EKS 클러스터 상태를 확인해줘"
"현재 실행 중인 Pod 목록을 보여줘"
"노드 그룹을 스케일 아웃해줘"
```

### 10. AWS ECS MCP Server

**역할:** 컨테이너 서비스 관리

**주요 API:**
- ECS 클러스터 관리
- 서비스 및 태스크 관리
- 컨테이너 로그 조회

**사용 예시:**
```
"ECS 서비스 상태를 확인해줘"
"실행 중인 태스크를 보여줘"
"컨테이너 로그를 확인해줘"
```

### 11. AWS Serverless MCP Server

**역할:** SAM 기반 서버리스 배포

**주요 API:**
- SAM 템플릿 생성
- 로컬 테스트
- 배포 관리

**사용 예시:**
```
"SAM 템플릿으로 Lambda 함수를 만들어줘"
"로컬에서 Lambda 함수를 테스트해줘"
"서버리스 애플리케이션을 배포해줘"
```

### 12. AWS Lambda Tool MCP Server

**역할:** Lambda 함수 실행 및 관리

**주요 API:**
- Lambda 함수 호출
- 함수 모니터링
- 프라이빗 리소스 접근

**사용 예시:**
```
"Lambda 함수를 실행해줘"
"Lambda 함수 로그를 확인해줘"
"함수 성능을 분석해줘"
```

## 사용 예시

### 인프라 관리

```
질문: "VPC와 EC2 인스턴스를 생성하는 CloudFormation 템플릿을 만들어줘"

AI 응답 예시:
CloudFormation 템플릿 생성:
- VPC (10.0.0.0/16)
- 퍼블릭 서브넷 2개
- 인터넷 게이트웨이
- 보안 그룹 (SSH, HTTP 허용)
- EC2 인스턴스 (t3.micro)
```

### 모니터링

```
질문: "현재 발생한 CloudWatch 알람을 분석하고 원인을 찾아줘"

AI 응답 예시:
발생한 알람: HighCPUUtilization
- 대상: i-1234567890abcdef0
- 현재 CPU: 95%
- 임계값: 80%

근본 원인 분석:
1. 최근 30분간 CPU 사용률 급증
2. 로그 분석 결과: 메모리 누수 의심
3. 프로세스: java 애플리케이션

권장 조치:
1. 애플리케이션 재시작
2. 힙 메모리 설정 확인
3. 인스턴스 타입 업그레이드 고려
```

### 보안 관리

```
질문: "S3 버킷에 읽기 전용 권한을 가진 IAM 역할을 만들어줘"

AI 응답 예시:
IAM 역할 생성:
- 역할 이름: S3ReadOnlyRole
- 신뢰 정책: EC2 서비스
- 권한 정책:
  * s3:GetObject
  * s3:ListBucket
```

### 컨테이너 관리

```
질문: "EKS 클러스터의 현재 상태와 문제점을 확인해줘"

AI 응답 예시:
클러스터: production-eks
- 상태: ACTIVE
- 버전: 1.28
- 노드 그룹: 3개 (총 12 노드)

발견된 문제:
1. 노드 2개가 NotReady 상태
   → 원인: 디스크 용량 부족
   → 조치: 불필요한 이미지 정리

2. Pod 5개가 Pending 상태
   → 원인: 리소스 부족
   → 조치: 노드 그룹 스케일 아웃
```

### 서버리스 배포

```
질문: "Python으로 S3 이벤트를 처리하는 Lambda 함수를 만들고 배포해줘"

AI 응답 예시:
SAM 템플릿 생성:
- 함수 이름: S3EventProcessor
- 런타임: Python 3.11
- 메모리: 256MB
- 타임아웃: 30초
- 트리거: S3 버킷 (ObjectCreated 이벤트)

배포 완료:
- 함수 ARN: arn:aws:lambda:us-east-1:123456789012:function:S3EventProcessor
```

### 로그 분석

```
질문: "지난 1시간 동안 애플리케이션 로그에서 에러를 찾아줘"

AI 응답 예시:
로그 그룹: /aws/ecs/my-app
분석 기간: 2024-02-06 14:00 - 15:00

발견된 에러:
1. NullPointerException (23건)
   - 발생 시간: 14:15, 14:32, 14:45
   - 영향: 사용자 로그인 실패

2. DatabaseConnectionTimeout (5건)
   - 발생 시간: 14:50 - 14:55
   - 영향: 데이터 조회 실패

권장 조치:
1. UserService 코드 수정
2. RDS 연결 풀 크기 증가
```

### 보안 감사

```
질문: "최근 24시간 동안 의심스러운 IAM 활동을 찾아줘"

AI 응답 예시:
CloudTrail 이벤트 분석:

⚠️ 주의 필요:
1. 비정상적인 권한 변경
   - 시간: 2024-02-06 02:30 AM
   - 사용자: admin-user
   - 작업: PutUserPolicy
   - IP: 203.0.113.45 (해외 IP)

권장 조치:
1. admin-user 계정 즉시 비활성화
2. MFA 강제 활성화
3. IP 화이트리스트 설정
```

## IAM 권한 설정

### 필요한 IAM 권한

AWS Console → IAM → Policies → Create policy → JSON 탭에 아래 내용 붙여넣기:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AWSDevOpsPermissions",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "cloudwatch:*",
        "logs:*",
        "ec2:Describe*",
        "ecs:*",
        "eks:*",
        "lambda:*",
        "iam:Get*",
        "iam:List*",
        "s3:*",
        "cloudtrail:LookupEvents",
        "aps:*"
      ],
      "Resource": "*"
    }
  ]
}
```

### 정책 적용 방법

1. **정책 생성**
   - 정책 이름: `AWSDevOpsMCPServerPolicy`
   - Create policy 클릭

2. **사용자에게 연결**
   - IAM → Users → 사용자 선택
   - Permissions → Add permissions
   - Attach policies directly
   - `AWSDevOpsMCPServerPolicy` 검색 후 선택
   - Add permissions

### AWS CLI로 적용

```bash
# 정책 생성
aws iam create-policy \
  --policy-name AWSDevOpsMCPServerPolicy \
  --policy-document file://iam_policy_devops_mcp.json

# 사용자에게 연결
aws iam attach-user-policy \
  --user-name YOUR_USERNAME \
  --policy-arn arn:aws:iam::ACCOUNT_ID:policy/AWSDevOpsMCPServerPolicy
```

## 문제 해결

### MCP 서버가 연결되지 않을 때

**1. uvx 설치 확인**
```bash
uvx --version
```

**2. AWS 자격증명 확인**
```bash
aws sts get-caller-identity
```

**3. MCP 서버 수동 테스트**
```bash
# 각 서버를 개별적으로 테스트
uvx awslabs.core-mcp-server@latest
uvx awslabs.cloudwatch-mcp-server@latest
uvx awslabs.iac-mcp-server@latest
```

**4. AI 에디터 재시작**
- 에디터 완전히 종료
- 터미널도 새로 열기
- 에디터 다시 시작

### 권한 오류가 발생할 때

**에러 메시지:**
```
AccessDeniedException: User is not authorized to perform: cloudformation:DescribeStacks
```

**해결 방법:**
1. IAM 정책이 올바르게 적용되었는지 확인
2. 정책 전파 대기 (1-2분)
3. AWS 자격증명 새로고침
```bash
aws configure
```

### 로그 확인

문제 해결을 위해 로그 레벨 변경:

```json
{
  "env": {
    "FASTMCP_LOG_LEVEL": "DEBUG"
  }
}
```

## 추가 리소스

- [AWS DevOps 블로그](https://aws.amazon.com/blogs/devops/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS MCP 서버 GitHub](https://github.com/awslabs/mcp)
- [MCP 프로토콜 문서](https://modelcontextprotocol.io/)

---

## License

This library is licensed under the MIT-0 License. See the [LICENSE](./LICENSE) file.
