# 곽경국 포트폴리오

https://github.com/haroogram/anonymous_project

https://m.site.naver.com/20OAJ

### 🧑‍💻 자기소개

저는 단기적인 성과보다,
오래 유지될 수 있는 구조와 지속 가능한 업무 방식을 선호합니다.

이 포트폴리오는 
”***AWS 기반 교육형 웹 서비스 프로젝트”***를 진행하며
확인하고 정리하는 과정에서 얻은 판단의 
기록입니다.

![image.png](image.png)

---

### 목차

1. 프로젝트 개요
2. 문제 정의 및 트러블 슈팅
3. 성과 및 개선
4. 배운점

---

### 프로젝트 개요

웹 기능 구현이 아닌 AWS 인프라 아키텍처 설계·구축·보안·CI/CD·IaC·운영 자동화 역량 검증을 목적으로 바이브 코딩(AI)으로 애플리케이션 최소 구현, 설계·의사결정·협업 프로세스에 집중한 프로젝트

- 담당 역할 : 팀장
- 담당 업무 : 회의 진행, 산출물 작성(요구사항 정의서, ERD, 어플리케이션 아키텍처 다이어그램, WireFrame, WBS), AI 기반 Django 웹 어플리케이션 개발, CI/CD 구축

개발 환경

- AWS Cloud Infrastructure
- Ubuntu (EC2 기반 서버 환경)
- Multi-AZ Architecture
- Auto Scaling Environment
- IaC 기반 인프라 구성

사용 도구

- Git / GitHub
- GitHub Actions
- AWS
- Cursor

사용 기술

- Python / Django
- MariaDB (AWS RDS)
- Redis (Cache / Broker)
- Celery / Celery Beat
- AWS (EC2, RDS, S3, IAM, WAF, CloudFormation)

---

### 문제 정의 및 트러블 슈팅

1. Github Action에서 Packer를 사용한 AMI build 및 배포 시 시간이 오래 걸림.
    - BEFORE ⇒ AWS ASG 환경에서 Github Action을 사용해 Packer로 AMI를 build해서 ASG RollingUpdate로 배포
    - AFTER ⇒ Packer는 최초 OS, 기본 package 수준의 인프라 표준화 용도로만 사용하고, 앱 배포는 Github Action에서 패키지 build 후 S3 upload → Code Deploy로 배포
    - RESULT ⇒ packer를 사용한 20분 이상 걸리던 배포 구조를 변경하여 Code Deploy를 통해 5분 이내로 감소시켰습니다.

1. Cloud Watch를 통한 모니터링 설정 중 EC2 CPU가 99%까지 올랐다가 10%대로 제한되다가 ASG에 의해 반복적으로 재생성되는 상황 발견.
    - BEFORE ⇒ EC2는 FreeTier 인 t3.micro 타입을 사용하여 상당히 낮은 사양인 상태에서 SSM, Cloud Watch, Code Deploy agent 등을 사용하여 과도한 부하로 CPU가 baseline인 10%로 제한됨.
    - AFTER ⇒ EC2 타입 상향 or 오버 엔지니어링 요소 제거
    - RESULT ⇒ 운영 관점에서 오버 엔지니어링이 오히려 장애를 유발할 수 있다는 점을 깨달았습니다.

1. R&R을 분배하여 각자 파트를 작업하던 중 CI/CD 테스트 후 점검 중 갑자기 웹 어플리케이션의 /admin 경로 접근이 안되는 상황 발생.
    - BEFORE ⇒ 동일 서버 내 python 및 django framework를 사용한 
                      웹 어플리케이션 + nginx 구성 → ALB나 Routing은 다 정상이었음 
                 → 403 오류를 기반으로 원인 추적 결과 보안 파트에서 WAF에 ACL Ruleset을 설정하면서 /admin 경로가 자동으로 차단됨.
    - AFTER ⇒ WAF ACL Ruleset에서 **`AWSManagedRulesAdminProtectionRuleSet`** 설정 예외 처리
    - RESULT ⇒ IaC를 통한 명확한 관리의 필요성과 단순히 배포가 성공했다고 해서 서비스가 정상이라고 볼 수 없다는 점을 알게 되었습니다. 특히 보안 설정이나 AWS 서비스 구성 오류처럼 인프라 레벨의 문제는 실제 트래픽이 유입되기 전까지 드러나지 않는 경우를 발견하였고 이런 리스크를 줄이기 위해, 인프라·보안 설정까지 포함해 실제 동작을 검증한 후 트래픽을 전환할 수 있는 블루/그린 배포 방식의 필요성을 느꼈습니다.

---

### 성과 및 개선

- 기본적인 AWS 인프라 설계 및 구축 경험 확보
- IaC 기반 협업과 CI/CD 자동화 경험
- 팀 회의와 공통 문서 작성, 노션 정리 및 정보 공유를 통한 협업 역량 강화
- 바이브 코딩을 활용한 어플리케이션 최소 구현 및 설계 협업 달성

- SPOF 상태인 Redis Server(EC2)를 ElastiCache 서비스로 전환
- 인프라적인 요소까지 고려한 Blue/Green 배포 전략 수립
- IAM Role 기반 임시권한 표준으로 전환하여 보안 강화
- 무조건적인 비용 최적화가 아닌 리소스 특성과 워크로드에 맞는 선택 필요

---

### 배운점

AWS Cloud 인프라를 전반적으로 구축하고 CI/CD 자동화 세팅을 하는 경험 자체는 웹 서비스의 가장 큰 라이프 싸이클을 다 볼 수 있어서 아주 좋았습니다.

다만, AI를 사용한 바이브 코딩으로 빠르게 웹 서비스를 개발하는 과정에서 선 구현 후 학습의 형태를 경험하였고 사람이 AI의 학습 속도를 따라가긴 어렵다는 것을 느꼈습니다.

하지만 의사결정의 주체로서 신뢰와 책임을 갖는 사람이 되려면 여전히 기본적인 지식과 경험에 따른 지혜는 갖추어야 AI를 제대로 활용할 수 있겠다는 생각이 들었습니다.
