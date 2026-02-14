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
웹 기능 개발이 아닌, AWS 인프라 아키텍처 설계·구축·보안·CI/CD·운영 자동화 역량 검증

**Role**
- 팀장
- 요구사항 정의서 / ERD / 아키텍처 다이어그램 / WBS 작성
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
(내용 동일)

### 2️⃣ t3.micro CPU Throttling 문제
(내용 동일)

### 3️⃣ WAF Ruleset으로 인한 /admin 403 오류
(내용 동일)

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
- AI 활용 시 설계 책임은 엔지니어에게 있다는 점 인지

---

## 🔗 Links

- GitHub: https://github.com/haroogram/anonymous_project
- Demo PPT: https://m.site.naver.com/20OAJ
