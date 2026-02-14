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

## 🛠 Technology (보유 기술)

| 기술분류 | 보유기술 |
|----------|----------|
| **네트워크 / 인프라 기초** | • TCP/IP, DNS, DHCP 등 네트워크 기본 구조 이해<br>• L2/L3 스위칭·라우팅 구조 이해 및 실습 경험<br>• VPN 환경에서 내부 네트워크 접근 및 서비스 연결 경험<br>• Wireshark 활용 패킷 분석 및 장애 원인 파악<br>• ACL 기반 트래픽 제어 및 기본적인 네트워크 보안 설정 경험 |
| **서버 / OS 운영** | • Linux(Ubuntu) 환경에서 서버 구축 및 운영<br>• Nginx 기반 웹 서버 구성 및 서비스 운영 경험<br>• MariaDB(MySQL) 설치·연동 및 애플리케이션 연동 경험<br>• Web / Mail / DHCP / FTP 등 기본 인프라 서비스 구성 경험<br>• 서버 이중화 구조 이해 및 실습 경험 |
| **가상화 / 클라우드** | • VMware 기반 가상화 환경에서 VM 구성 및 관리<br>• AWS 환경에서 다음 서비스 활용 경험<br>&nbsp;&nbsp;&nbsp;&nbsp;◦ EC2, S3, RDS, IAM, Auto Scaling, Code Deploy<br>• 클라우드 리소스 간 연동 구조 이해 (네트워크·보안·권한 중심) |
| **컨테이너 / 배포 환경** | • Docker를 활용한 애플리케이션 이미지 빌드 및 배포 경험<br>• Docker 기반 서비스 구성 및 로컬/서버 환경 실행<br>• 컨테이너 기반 환경에서 애플리케이션 구조 이해 |
| **Kubernetes** | • Kubernetes 기본 아키텍처 이해 (Master / Worker 구조)<br>• Minikube 기반 로컬 Kubernetes 환경 구축<br>• Deployment, Service, Pod 단위 리소스 운영 경험<br>• Rollout, Rollback, 기본 트래블슈팅 경험<br>• 장애 발생 시 로그·상태 기반으로 원인 추적 경험 |
| **모니터링 / 운영 관점** | • 기본적인 모니터링 구조 이해<br>• Prometheus & Grafana 활용 경험<br>• 서비스 상태 및 리소스 사용량 확인 경험<br>• 장애 발생 시 로그·상태 기반으로 원인 범위 축소 경험 |

<!-- 
이 영역에는 개인의 배경, 경력 요약, 기술적 정체성,
DevOps/Infra를 선택하게 된 이유 등을 작성 예정
-->

---

## 🚀 Project Overview

### 📌 AWS 기반 교육형 웹 서비스 프로젝트

**목표**  
웹 기능 개발이 아닌, AWS 인프라 아키텍처 설계·구축·보안·CI/CD·운영 자동화 역량 검증

본 프로젝트는 애플리케이션 기능 구현보다  
인프라 설계, IaC 기반 환경 구성, 배포 자동화,  
보안 설정 및 운영 이슈 대응까지  
서비스의 전 라이프사이클을 직접 설계·검증하는 것을 목표로 진행되었습니다.

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

## 🏗 Architecture & Environment

### Cloud Environment
- AWS Multi-AZ Architecture
- Auto Scaling Group
- ALB
- WAF
- IAM Role 기반 접근 제어

### Server
- Ubuntu (EC2)
- MariaDB (RDS)
- Redis (EC2 → ElastiCache 전환)

### CI/CD
- GitHub Actions
- S3
- CodeDeploy
- CloudFormation (IaC)

### Tech Stack
- Python / Django
- Redis
- Celery / Celery Beat
- AWS (EC2, RDS, S3, IAM, WAF, CloudWatch, CloudFormation)

---

## 🧠 Trouble Shooting & Engineering Decisions

### 1️⃣ Packer 기반 AMI 배포 시간 과다 문제

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

### 2️⃣ t3.micro CPU Throttling 문제

**현상**
- CPU 99% → 10% 제한 반복
- ASG에 의해 인스턴스 재생성

**원인**
- FreeTier t3.micro baseline 10%
- SSM / CloudWatch / CodeDeploy Agent 부하

**해결**
- 인스턴스 타입 상향
- 오버엔지니어링 요소 제거

**교훈**
- 비용 최적화 ≠ 무조건 저사양
- 워크로드 기반 리소스 설계 필요

---

### 3️⃣ WAF Ruleset으로 인한 /admin 403 오류

**현상**
- ALB 정상
- Routing 정상
- /admin 접근 시 403 발생

**원인**
- WAF `AWSManagedRulesAdminProtectionRuleSet` 적용으로 자동 차단

**조치**
- ACL Ruleset 예외 처리

**교훈**
- 배포 성공 ≠ 서비스 정상
- 보안 설정 포함 인프라 레벨 검증 필요
- Blue/Green 배포의 필요성 인지

---

## 📈 Improvements

- Redis EC2 → ElastiCache 전환 (SPOF 제거)
- Blue/Green 배포 전략 설계
- IAM Role 기반 임시 권한 구조 적용
- IaC 기반 협업 프로세스 확립

---

## 📚 What I Learned

- AWS 인프라 전체 라이프사이클 경험
- IaC + CI/CD 자동화 설계 경험
- 인프라 레벨 장애가 서비스에 미치는 영향 체감
- AI를 활용하되, 의사결정은 사람이 책임져야 한다는 점

> AI는 도구이고,  
> 설계와 판단의 책임은 엔지니어에게 있습니다.

---

## 🔗 Links

- GitHub: [AWS 기반 교육형 웹 서비스 프로젝트](https://github.com/haroogram/anonymous_project)
- Demo PPT: [AWS 기반 교육형 웹 서비스 프로젝트 PPT](https://m.site.naver.com/20OAJ)
