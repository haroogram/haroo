# 👋 안녕하세요, 곽경국입니다.

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

AWS 기반 인프라 설계 및 운영 자동화 역량을 검증하기 위해  
교육형 웹 서비스 프로젝트를 기획·설계·구축했습니다.

이 프로젝트는 단순 기능 구현이 아니라,

- 인프라 아키텍처 설계
- IaC 기반 환경 구성
- CI/CD 자동화
- 보안 설정 및 운영 이슈 대응

까지 전 과정을 직접 경험하고 검증하는 것을 목표로 진행되었습니다.

---

## 🚀 Project Overview

### 📌 AWS 기반 교육형 웹 서비스 프로젝트

**목표**  
웹 기능 개발이 아닌 AWS 인프라 아키텍처 설계·구축·보안·CI/CD·운영 자동화 역량 검증

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

### 1️⃣ Github Action에서 Packer를 사용한 AMI build 및 배포 시 시간이 오래 걸림

**BEFORE**
- AWS ASG 환경에서 Github Action을 사용해 Packer로 AMI를 build해서 ASG RollingUpdate로 배포

**AFTER**
- Packer는 최초 OS, 기본 package 수준의 인프라 표준화 용도로만 사용
- 앱 배포는 Github Action에서 패키지 build 후 S3 upload → Code Deploy로 배포

**RESULT**
- packer를 사용한 20분 이상 걸리던 배포 구조를 변경하여 Code Deploy를 통해 5분 이내로 감소

---

### 2️⃣ CloudWatch 모니터링 중 EC2 CPU 99% → 10% 제한 반복 재생성 문제

**BEFORE**
- EC2는 FreeTier t3.micro 타입 사용
- SSM, CloudWatch, CodeDeploy agent 사용으로 과도한 부하
- CPU baseline 10% 제한

**AFTER**
- EC2 타입 상향
- 오버 엔지니어링 요소 제거

**RESULT**
- 운영 관점에서 과도한 비용 절감 또는 오버엔지니어링이 오히려 장애를 유발할 수 있다는 점 인지

---

### 3️⃣ CI/CD 테스트 후 /admin 403 오류 발생

**상황**
- 동일 서버 내 Python/Django + Nginx 구성
- ALB 및 Routing 정상
- /admin 경로 접근 불가

**원인**
- WAF ACL Ruleset 설정 중 `AWSManagedRulesAdminProtectionRuleSet` 적용으로 /admin 자동 차단

**AFTER**
- WAF ACL Ruleset 예외 처리

**RESULT**
- IaC 기반 명확한 관리의 필요성 인지
- 배포 성공 ≠ 서비스 정상
- 인프라·보안 설정까지 포함한 검증 필요
- Blue/Green 배포 방식의 필요성 체감

---

## 📈 Improvements

- Redis EC2 → ElastiCache 전환 (SPOF 제거)
- 인프라 요소까지 포함한 Blue/Green 배포 전략 수립
- IAM Role 기반 임시 권한 표준 전환
- 리소스 특성과 워크로드 기반 설계 필요성 인지

---

## 📚 What I Learned

- AWS 인프라 전반적인 라이프사이클 경험
- IaC 기반 협업과 CI/CD 자동화 경험
- 인프라 레벨 장애 리스크 체감
- AI는 도구이며, 의사결정과 책임은 엔지니어에게 있다는 점

---

## 🔗 Links

- GitHub: https://github.com/haroogram/anonymous_project
- Demo PPT: https://m.site.naver.com/20OAJ
