# 🎙️ DialoG - STT 기반 실시간 회의록 자동화 서비스 (Monorepo)

![Project Status](https://img.shields.io/badge/Status-Completed-success) ![Java](https://img.shields.io/badge/Java-17-orange) ![SpringBoot](https://img.shields.io/badge/SpringBoot-3.x-green) ![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)

> **✨ 백엔드 핵심 역량 요약 (Recruiter's Pick):**
> 
> **1. 보안 아키텍처:** Spring Security 기반 JWT 인증을 **HttpOnly Cookie**로 강화하여 XSS 공격을 원천 차단했습니다.
> **2. 데이터 정합성:** Google Calendar 연동 시 발생하는 **'Zombie Data(삭제된 일정 부활)' 버그를 해결**하며 데이터 무결성을 확보했습니다.
> **3. 운영 안정성:** Docker 배포 환경의 변수 격리 문제를 해결하고 **RBAC 기반 권한 관리 체계**를 구축했습니다.

---

## 🔗 포트폴리오 및 데모 (Links)

| 구분 | 주소 | 비고 |
| :--- | :--- | :--- |
| **상세 포트폴리오** | [**👉 Notion 포트폴리오 바로가기**](https://www.notion.so/DialoG-STT-2bb357308ca280f297c6f86b0faba013?source=copy_link) | **개발 일지, 트러블 슈팅 상세 내역 (필수 확인)** |
| **시연 영상** | [유튜브 링크 입력] | 서비스 핵심 기능 시연 |
| **배포 URL** | [배포 주소 입력] | 실제 서비스 접속 |

---

## 📅 프로젝트 개요 (Overview)

* **프로젝트명:** DialoG (다이얼로그)
* **한 줄 소개:** 실시간 음성 인식(STT)을 통해 회의록을 자동 작성하고, AI 요약 및 캘린더 연동을 제공하는 업무 생산성 도구
* **진행 기간:** 2025.09.29 ~ 2025.11.24 (약 2개월)
* **참여 인원:** 5명 (학원 프로젝트 / Full-stack)
* **담당 역할:**
    * **Backend:** REST API 설계, DB 모델링, 인증/인가 보안 구축, 외부 API 연동
    * **DevOps:** Docker 컨테이너 환경 구축 및 배포, Nginx 설정
    * **Frontend:** Vanilla JS 기반의 SPA 구조 설계 및 UI 구현

---

## 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 |
| :--- | :--- |
| **Backend** | Java 17, Spring Boot 3.x, Spring Security, JPA (Hibernate) |
| **AI / Data** | Python (FastAPI), Naver Clova (STT, Summary, Sentiment) |
| **Database** | MySQL 8.0, Redis (Optional) |
| **DevOps** | Docker, Docker Compose, AWS EC2 (예정) |
| **Frontend** | JavaScript (ES6+), HTML5, CSS3, Fetch API |
| **Tools** | Git, GitHub, Notion, Postman |

---

## 🔥 핵심 기술적 챌린지 (Key Troubleshooting)
> 프로젝트를 진행하며 마주친 기술적 난관과 이를 해결하며 얻은 인사이트입니다.

### 1. 🛡️ XSS 방어를 위한 인증 구조 개선 (Security)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | 초기 `LocalStorage`에 JWT를 저장하는 방식은 **XSS(Cross-Site Scripting)** 공격에 취약하여 토큰 탈취 위험이 높음. |
| **해결 (Solution)** | **HttpOnly Cookie** 도입으로 자바스크립트의 토큰 접근을 차단하고, 서버 측 로그아웃 로직(Refresh Token 삭제)을 구현하여 보안성을 강화함. |
| **성장 (Growth)** | 편의성보다 **보안이 우선되는 설계**의 중요성을 체감하고, Stateless한 환경에서의 세션 제어 방법을 익힘. |

### 2. 🧟 Google Calendar 'Zombie Data' 동기화 문제 (Data Integrity)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | Google Calendar에서 삭제된 일정이 로컬 DB 동기화 시 계속 조회되는 현상 발생. |
| **원인 (Cause)** | Google API 응답 중 `status: cancelled` (삭제됨) 상태 필드를 DTO 매핑에서 누락하여 필터링 실패. |
| **해결 (Solution)** | API 응답 DTO에 상태 필드를 추가하고, 비즈니스 로직에서 **Cancelled 상태를 필터링**하여 뷰 렌더링을 원천 차단함. |
| **성장 (Growth)** | 외부 API 연동 시 **문서(Docs)의 모든 필드를 방어적으로 검토**해야 함을 배웠으며, 데이터 정합성 유지의 중요성을 깨달음. |

### 3. 🐳 Docker 환경 격리 및 배포 트러블 슈팅 (DevOps)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | 로컬(`localhost`)과 Docker 컨테이너 환경 간의 **OAuth Redirect URI 불일치**로 인해 400 에러 발생. |
| **해결 (Solution)** | `docker-compose.yml`을 통해 **환경 변수(Environment Variables)를 분리 주입**하여, 배포 환경에 맞는 Callback URI가 동적으로 적용되도록 구성 관리 전략 수립. |
| **성장 (Growth)** | **'내 컴퓨터에서는 되는데'** 문제를 해결하며 컨테이너 네트워크와 환경 변수 관리의 필요성을 체득함. |

---

## 📂 서비스 구성 및 폴더 구조 (Monorepo)

이 프로젝트는 효율적인 관리를 위해 Backend, Frontend, AI 서버가 통합된 **Monorepo** 구조입니다.

| 모듈 | 경로 | 설명 |
| :--- | :--- | :--- |
| **Backend** | [`/backend`](./backend) | **Spring Boot** 기반의 메인 API 서버 (인증, 비즈니스 로직) |
| **Frontend** | [`/frontend`](./frontend) | **Vanilla JS** 기반의 사용자 인터페이스 및 소셜 로그인 처리 |
| **AI Server** | [`/ai`](./ai) | **Python FastAPI** 기반의 STT 후처리 및 요약 서버 |

```bash
DialoG-Project/
├── backend/       # Spring Boot Source Code
├── frontend/      # Vanilla JS Source Code
├── ai/            # Python Source Code
├── docker-compose.yml # 통합 실행 설정
├── .env           # 환경 변수 (Ignored)
└── README.md      # Project Documentation
