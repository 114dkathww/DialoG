# 🎙️ DialoG - STT 기반 실시간 회의록 자동화 서비스 (Monorepo)

![Project Status](https://img.shields.io/badge/Status-Completed-success) ![Java](https://img.shields.io/badge/Java-17-orange) ![SpringBoot](https://img.shields.io/badge/SpringBoot-3.x-green) ![Spring Security](https://img.shields.io/badge/Spring_Security-6.x-red) ![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)

> **✨ 백엔드 핵심 역량 요약 (Recruiter's Pick):**
>
> **1. 보안 아키텍처:** JWT를 **HttpOnly Cookie**에 저장하고 **Refresh Token Rotation**을 적용하여 XSS 공격을 원천 차단했습니다.
> 
> **2. 데이터 정합성:** Google Calendar 연동 시 **'Zombie Data(삭제된 일정 부활)' 버그를 응답 상태 필터링으로 해결**하고, Cascade 삭제 로직으로 무결성을 확보했습니다.
> 
> **3. 쿼리 최적화:** 관리자 대시보드의 일별 통계 산출을 위해 **JPQL 범위 검색(Range Scan)**을 적용하고, DB 레벨 집계로 애플리케이션 부하를 최소화했습니다.

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
    * **Backend (Core):**
        * **Security:** Spring Security & OAuth2(Google/Kakao) 기반 인증/인가 시스템 설계 및 구현
        * **API & Logic:** 구글 캘린더 양방향 동기화, 회의록 생성 및 관리자 통계 시스템(JPQL) 개발
        * **Stability:** Global Exception Handler 도입 및 SMTP 비밀번호 재설정 기능 구현
    * **Frontend:**
        * 인증 관련 UI(로그인/회원가입/비밀번호 찾기) 및 관리자 대시보드(Chart.js) 데이터 연동

---

## 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 |
| :--- | :--- |
| **Backend** | Java 17, Spring Boot 3.x, Spring Security 6, JPA (Hibernate) |
| **AI / Data** | Python (FastAPI), Naver Clova (STT, Summary, Sentiment) |
| **Database** | MySQL 8.0, Redis (Token Caching) |
| **Frontend** | JavaScript (ES6+), HTML5, CSS3, Fetch API |
| **Tools** | Git, GitHub, Notion, Postman |

---

## 🔥 핵심 기술적 챌린지 (Key Troubleshooting)
> 프로젝트를 진행하며 마주친 기술적 난관과 이를 해결하며 얻은 인사이트입니다.

### 1. 🛡️ XSS 방어를 위한 인증 구조 고도화 (Security)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | 초기 `LocalStorage` 저장 방식은 XSS에 취약하며, 일반/소셜 로그인 간의 인증 객체 타입(`UserDetails` vs `OAuth2User`) 불일치로 통합 관리가 어려움. |
| **해결 (Solution)** | **HttpOnly Cookie** 도입으로 JS 접근을 차단하고, `CustomUserDetails`로 이종 인증 객체를 통합함. 추가로 **Refresh Token Rotation**을 구현해 보안성을 극대화함. |
| **성장 (Growth)** | 편의성보다 **보안이 우선되는 설계**의 중요성을 체감하고, Stateless 환경에서의 쿠키 속성(HttpOnly, Secure) 활용법을 익힘. |

### 2. 🧟 Google Calendar 'Zombie Data' 동기화 문제 (Data Integrity)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | Google Calendar에서 삭제된 일정이 로컬 DB 동기화 시 API 응답에 포함되어 계속 부활하는 현상 발생. |
| **원인 (Cause)** | Google API 응답 중 `status: cancelled` (삭제됨) 상태 필드를 DTO 매핑에서 누락하여 비즈니스 로직에서 필터링 실패. |
| **해결 (Solution)** | API 응답 DTO에 상태 필드를 추가하여 **Cancelled 상태를 필터링**하고, 회의 삭제 시 연관 데이터(참여자, 일정)를 **Cascade로 정리**하여 무결성을 확보함. |
| **성장 (Growth)** | 외부 API 연동 시 **문서(Docs)의 응답 필드를 방어적으로 검토**해야 함을 배웠으며, 데이터 정합성 유지를 위한 예외 처리의 중요성을 깨달음. |

### 3. 📊 관리자 대시보드 날짜/통계 처리 트러블 슈팅 (Data Processing)
| 구분 | 내용 |
| :--- | :--- |
| **문제 (Issue)** | 관리자 대시보드 통계 집계 시, Java `Date` 객체의 Timezone 차이로 날짜가 하루씩 밀리거나 전일 대비 증감률 쿼리가 복잡해지는 문제. |
| **해결 (Solution)** | `LocalDateTime`의 `atStartOfDay()`를 활용해 정확한 **범위 검색(Range Scan)**을 구현하고, **JPQL로 통계 로직을 최적화**하여 DB 레벨에서 데이터를 산출함. |
| **성장 (Growth)** | 정확한 데이터 분석을 위한 **Timezone 처리** 전략과, 대용량 데이터 처리를 고려한 **쿼리 최적화**의 효율성을 체득함. |

---

## 📂 서비스 구성 및 폴더 구조 (Monorepo)

이 프로젝트는 효율적인 관리를 위해 Backend, Frontend, AI 서버가 통합된 **Monorepo** 구조입니다.

| 모듈 | 경로 | 설명 |
| :--- | :--- | :--- |
| **Backend** | [`/backend`](./backend) | **Spring Boot** 기반의 메인 API 서버 (인증, 비즈니스 로직, 관리자) |
| **Frontend** | [`/frontend`](./frontend) | **Vanilla JS** 기반의 사용자 인터페이스 및 소셜 로그인 처리 |
| **AI Server** | [`/ai`](./ai) | **Python FastAPI** 기반의 STT 후처리 및 요약 서버 |

```bash
DialoG-Project/
├── backend/       # Spring Boot Source Code
├── frontend/      # Vanilla JS Source Code
├── ai/            # Python Source Code
├── .env           # 환경 변수 (Ignored)
└── README.md      # Project Documentation
