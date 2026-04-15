> 구조적이고 지속 가능한 인프라를 설계하는 엔지니어  
> 단기적 결과보다 “오래 유지될 수 있는 시스템”을 지향합니다.

---

## 📌 Table of Contents

1. [About Me](#-about-me)
2. [Project Overview](#-project-overview)
3. [Architecture & Environment](#-architecture--environment)
4. [Trouble Shooting & Engineering Decisions](#-trouble-shooting--engineering-decisions)
5. [Improvements](#-improvements)
6. [What I Learned](#-what-i-learned)
7. [Links](#-links)

---

## 🧑‍💻 About Me

> 👋 지속 가능한 인프라를 설계하는 **엔지니어 곽경국**입니다.

### 🛠 Technical Skills

**Infra / Network**  
TCP/IP · DNS · ACL · VPN · Routing  

**Server / DB**  
Linux(Ubuntu) · Nginx · Apache · MySQL · Oracle

**VM / Cloud**  
VM ware · AWS (EC2 · RDS · IAM · ASG · WAF · CodeDeploy)  

**Container & Orchestration**  
Docker · Kubernetes · Minikube  

**DevOps / Automation**  
Git · GitHub Actions · Terraform · CloudFormation  

**Application**  
Java · Spring Boot · Python · Django · PHP · REST API 

**Monitoring**  
Prometheus · Grafana


<!-- 
이 영역에는 개인의 배경, 경력 요약, 기술적 정체성,
DevOps/Infra를 선택하게 된 이유 등을 작성 예정
-->

---

## 🔖 Project Overview

### AWS 기반 교육형 웹 서비스 프로젝트 (v1) → AWS 인프라 고도화 프로젝트 (v2)

본 포트폴리오는 **서로 연계된 2개의 프로젝트**를 함께 다룹니다.  
v1에서 서비스 기반 인프라를 설계·구축했고,  
v2에서 동일 도메인을 대상으로 인프라 운영/보안/IaC 구조를 고도화했습니다.

---

### 1️⃣ v1 - 서비스 구축 프로젝트

**목표**  
웹 기능 개발이 아닌, AWS 인프라 아키텍처 설계·구축·보안·CI/CD·운영 자동화 역량 검증

**Project Info**
- 프로젝트 기간 : 2025.12.17 ~ 2026.01.16 (총 31일)
- 참여 인원 : 4명

**Role**
- 팀장
- 회의 진행
- 요구사항 정의서 / ERD / 아키텍처 다이어그램 / WireFrame / WBS 작성
- AI 기반 Django 애플리케이션 개발
- CI/CD 구축

---

### 2️⃣ v2 - 인프라 고도화 프로젝트 (후속)

**목표**  
v1 운영 경험을 기반으로, 배포 안정성·보안 통제·인프라 운영 체계를 실무형 구조로 고도화

**Project Info**
- 프로젝트 기간 : 2026.03.03 ~ 2026.04.01 (총 30일)
- 참여 인원 : 1명

**Role**
- CloudFormation → Terraform 전환 설계 및 적용
- Terraform 모듈 구조(`base` / `data` / `app`) 설계
- 인프라 코드 전용 GitHub Private Repository 분리 운영
- GitHub Actions + Docker + K3s + Argo CD 기반 GitOps 파이프라인 구성
- Prometheus/Grafana 모니터링 체계 구축
- IAM/SSM/VPC Endpoint/WAF 기반 보안 통제 강화

---

## 👉 Architecture & Environment

### Current Architecture (v2)
![architecture-v2](images/architecture_update.png)

> v2는 **Terraform 모듈 분리(base/data/app) + K3s + Argo CD(GitOps)** 중심으로 재구성했습니다.

<details>
<summary>Legacy Architecture (v1) 보기</summary>

<img src="images/architecture.png" alt="architecture" />

</details>

### GitOps 운영 화면 (Argo CD)
![argocd-overview](images/argocd.png)

> Argo CD를 통해 `Healthy / Synced` 상태를 기준으로 배포 상태를 관리했습니다.

### Monitoring Dashboard (Grafana)
![grafana-dashboard](images/grafana.png)

> 노드/서비스 지표를 기반으로 성능 및 이상 징후를 모니터링했습니다.

---

### Cloud Environment (v2)
- AWS VPC (Public / Private App / Private DB subnet 분리) / Multi-AZ(Optional)
- ALB + WAFv2(CloudFront) + 관리 UI 화이트리스트
- IAM Role + SSM Parameter Store 기반 접근/시크릿 관리
- SSM / EC2Messages / KMS / Logs VPC Endpoint 기반 사설망 중심 운영
- SES + Route53(DKIM/SPF) 기반 메일 송신 및 인증 연동
  
<details>
<summary>Legacy Cloud Environment (v1) 보기</summary>
 
- AWS Multi-AZ VPC (Public / Private App / Private DB subnet 분리)
- ALB + WAFv2 + NAT
- IAM Role + SSM Parameter Store 기반 접근/시크릿 관리

</details>

---

### Server (v2)
- Ubuntu (EC2 t3.small) 
- K3s 클러스터 기반 컨테이너 배포
- MariaDB (RDS)
- Redis (Single EC2)
- Monitoring EC2 (Prometheus + Grafana)

<details>
<summary>Legacy Server (v1) 보기</summary>

- Ubuntu (EC2 t3.micro / ASG 기반 운영)
- 애플리케이션 직접 배포(CodeDeploy)
- MariaDB (RDS)
- Redis (Single EC2)

</details>

---

### CI/CD (v2)
- Terraform (base / data / app 모듈 분리)
- GitHub Actions: Docker Image 빌드 및 Docker Hub Push
- 태그 기반 릴리스 시 매니페스트 자동 업데이트
- Argo CD Sync 기반 GitOps 배포

<details>
<summary>Legacy CI/CD (v1) 보기</summary>

- CloudFormation 기반 인프라 구성
- GitHub Actions + AWS CodeDeploy 배포
- (초기) Packer AMI 기반 배포 운영 경험

</details>

---

### Tech Stack (v2)
- Python / Django
- Kubernetes (K3s), Traefik
- Argo CD, Terraform
- Prometheus / Grafana
- Redis(cache/broker), MariaDB
- Celery / Celery Beat

<details>
<summary>Legacy Tech Stack (v1) 보기</summary>

- Python / Django
- AWS CodeDeploy, CloudFormation
- (초기) Packer AMI 기반 배포
- Redis, MariaDB
- Celery / Celery Beat

</details>

---

## 🧠 Trouble Shooting & Engineering Decisions

### [v1] 1️⃣ Packer 기반 AMI 배포 시간 과다 문제

**BEFORE**
- GitHub Actions → Packer AMI Build
- ASG Rolling Update 방식 배포
- 평균 20분 이상 소요

**문제**
- AMI 단위 배포는 속도 및 운영 유연성이 낮음

**AFTER**
- Packer는 OS 표준화 용도로만 사용
- 앱은 GitHub Actions에서 빌드
- S3 업로드 → CodeDeploy 배포

**RESULT**
- 배포 시간 20분 → 5분 이내 단축
- 인프라 표준화와 애플리케이션 배포를 분리하는 구조 확립

---

### [v1] 2️⃣ t3.micro CPU Throttling 이슈

**BEFORE**
- Free Tier 환경에서 `t3.micro` 인스턴스 사용
- CPU 사용률 99% 도달 후 10%로 급격히 제한 반복
- ASG Health Check 실패로 인스턴스 재생성 발생

**문제**
- `t3.micro`의 Baseline CPU 10% 제한 구조
- SSM / CloudWatch / CodeDeploy Agent 등의 상시 부하 존재
- 실제 워크로드 대비 리소스 설계 부족

**AFTER**
- 인스턴스 타입 상향 조정
- 불필요한 Agent 및 오버엔지니어링 요소 제거
- 워크로드 기반 리소스 산정 기준 수립

**RESULT**
- CPU Throttling 현상 제거
- ASG 불필요 재생성 문제 해결
- 비용 최적화는 단순 저사양이 아닌 안정성 기반 설계라는 관점 확립

---

### [v1] 3️⃣ WAF Ruleset 으로 인한 `/admin` 경로 403 Error

**BEFORE**
- ALB 및 Target Group 정상 동작
- Routing 및 Health Check 이상 없음
- `/admin` 경로 접근 시 403 오류 발생

**문제**
- WAF `AWSManagedRulesAdminProtectionRuleSet` 적용으로 `/admin` 자동 차단
- 애플리케이션 레벨이 아닌 보안 정책 레벨 이슈

**AFTER**
- WAF ACL Ruleset 분석
- 해당 경로 예외 처리 적용
- 인프라 보안 정책 포함 검증 절차 수립

**RESULT**
- `/admin` 정상 접근 복구
- 배포 성공 ≠ 서비스 정상이라는 인식 확립
- 보안 설정까지 포함한 Blue/Green 배포 검증 필요성 도출

---
### [v2] 4️⃣ 관리 UI 서브도메인 분리를 통한 접속 개선

**BEFORE**
- Argo CD / Grafana 접속 시 매번 `port-forwarding`을 통해 로컬에서 접근
- 운영 도구 접근 절차가 번거롭고 속도가 느림

**문제**
- 관리 UI 접근성이 낮아 운영 대응 속도가 저하됨
- 직접 노출 시 보안 위험이 증가할 수 있어 접근 통제 기준이 필요함

**AFTER**
- 관리 UI를 서브도메인으로 분리해 직접 접근 구조로 전환
- `argocd.zero-one.click`
- `grafana.zero-one.click`
- WAF 화이트리스트 기반 접근 제한 적용
- 기본 SSO 절차 및 비밀번호 재설정 정책 반영

**RESULT**
- 운영 도구 접근 속도 및 사용성 개선
- 보안 통제(화이트리스트/인증 절차)와 운영 편의성 간 균형 확보
- 장애 대응 및 점검 시 초기 접근 리드타임 단축

---

### [v2] 5️⃣ OWASP Top 10 기준 기반 보안 점검 체계 도입

**BEFORE**
- 개발 시 보안 검토 기준이 프로젝트 내에서 명확히 정리되어 있지 않음
- 어떤 항목을 우선 점검해야 하는지 판단 근거가 부족함

**문제**
- 보안 요구사항이 암묵적으로 처리되어 누락 가능성이 존재
- 기능 개발과 보안 검증 사이의 기준 정렬이 어려움

**AFTER**
- OWASP Top 10을 기준으로 점검 항목을 정의하고 보안 검사 수행
- 기준 미달/미흡 항목은 보완 개발 진행
- 최종 단계에서 AI 기반 보안 검사 리포트 생성

**RESULT**
- 보안 검토 기준이 명문화되어 개발/검증 과정의 일관성 확보
- 잠재 취약점에 대한 사전 점검 체계 강화
- 보안 이슈를 사후 대응이 아닌 사전 예방 흐름으로 전환

---

### [v2] 6️⃣ Dependabot + Audit 자동화로 의존성 취약점 대응

**BEFORE**
- Docker 이미지 사용 중 기본 패키지에서 치명적 취약점 발견
- 수동 점검/수동 업데이트 중심으로 취약점 대응
- 의존성 변경 시점마다 보안 검증이 체계적으로 반복되지 않음

**문제**
- 패치 누락 및 대응 지연 가능성이 높음
- PR 단계에서 취약 의존성이 유입될 위험 존재

**AFTER**
- Dependabot 도입으로 취약 패치 버전 자동 PR 생성
- `pip-audit` 도입 및 월간 Scheduled 점검 수행
- `main` 브랜치 PR 시 `requirements.txt` 의존성 취약점 검사 파이프라인 적용

**RESULT**
- 취약점 탐지→패치 제안→검증의 자동화 루프 구축
- 의존성 보안 점검의 정기성과 재현성 확보
- 배포 전 단계에서 취약 의존성 유입 리스크 감소

---

## 📈 Improvements

- Redis EC2 → ElastiCache 전환 (SPOF 제거)
- Blue/Green 배포 전략
- 모니터링 알림 정책 및 웹훅 추가
- 관리 UI 관련 VPN 설정

---

## 📚 What I Learned

- AWS 인프라 전체 라이프사이클 경험
- IaC + CI/CD 자동화 설계 경험
- 인프라 레벨 장애가 서비스에 미치는 영향 체감
- AI를 활용하되, 의사결정은 사람이 책임져야 한다는 점
- 100%의 보안은 없다, 빠른 대응이 있을 뿐

> AI는 도구이고,  
> 설계와 판단의 책임은 엔지니어에게 있습니다.

---

## 🔗 Links

- [![GitHub](https://img.shields.io/badge/GitHub-Repository-black?logo=github)](https://github.com/haroogram/anonymous_project)
- [![Demo](https://img.shields.io/badge/Demo-PPT-blue)](https://m.site.naver.com/20OAJ)
 



