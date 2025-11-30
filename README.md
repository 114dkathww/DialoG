# DialoG
project(09.29~12.01)

🎙️ DialoG - STT 기반 실시간 회의록 자동화 서비스

"회의의 시작부터 끝까지, DialoG가 함께합니다."

실시간 음성 인식(STT)을 통해 회의 내용을 기록하고, AI 요약 및 액션 아이템을 추출하여 Google 캘린더에 자동 연동하는 업무 생산성 도구입니다.

📅 프로젝트 기간 및 인원

진행 기간: 2025.09.29 ~ 2025.11.24 (약 2개월)

참여 인원: 1명 (개인 프로젝트 / Full-stack)

📚 주요 기능 (Key Features)

1. 🔐 보안 및 인증 (Security & Auth)

OAuth2 소셜 로그인: Google, Kakao 연동 및 Factory Pattern을 적용한 사용자 정보 파싱 구조화.

JWT & HttpOnly Cookie: XSS 방어를 위해 Access Token을 HttpOnly Cookie에 저장하고, Refresh Token Rotation 적용.

RBAC 권한 관리: 일반 사용자(USER)와 관리자(ADMIN) 권한 분리 및 접근 제어.

2. 🎙️ 회의 자동화 (Meeting Automation)

실시간 STT: Naver Clova API를 활용한 실시간 음성 텍스트 변환.

AI 요약 및 키워드: 회의 내용 요약 및 핵심 키워드 추출.

캘린더 연동: 추출된 Action Item을 Google Calendar API를 통해 일정으로 자동 등록.

3. 📊 관리자 대시보드 (Admin Dashboard)

데이터 시각화: JPQL 통계 쿼리를 활용하여 전일 대비 증감률, 가입자 추이 등을 시각화.

데이터 무결성 보장: 회원/회의 삭제 시 연관 데이터(토큰, 참여 정보 등)를 안전하게 정리하는 Cascade 로직 구현.

🛠️ 기술 스택 (Tech Stack)

구분

기술

Backend

Java 17, Spring Boot 3, Spring Security, JPA(Hibernate)

Frontend

JavaScript(ES6+), HTML5, CSS3, Fetch API

Database

MySQL 8.0, Redis (Optional)

AI / Data

Python (FastAPI), Naver Clova (STT, Summary)

DevOps

Docker, Docker Compose, AWS EC2 (예정)

Collaboration

Git, GitHub, Notion

🏗️ 시스템 아키텍처 (Architecture)

<!-- 아키텍처 다이어그램 이미지가 있다면 여기에 넣으세요. 없다면 생략 가능 -->

🔥 핵심 기술적 챌린지 (Troubleshooting)

프로젝트를 진행하며 마주친 기술적 난관과 해결 과정입니다.

1. 👻 외부 API 데이터 정합성 문제 (Zombie Data)

문제: Google Calendar에서 삭제된 일정이 로컬 DB와 동기화되지 않아 계속 조회되는 현상 발생.

원인: Google API 응답의 status: cancelled 필드를 DTO에 매핑하지 않아 필터링 실패.

해결: 응답 DTO에 상태 필드를 추가하고, 조회 로직에서 Cancelled 상태를 필터링하여 뷰 렌더링을 원천 차단.

2. 🛡️ XSS 방어를 위한 인증 구조 개선

문제: LocalStorage에 JWT 저장 시 XSS 공격에 취약함.

해결:

HttpOnly Cookie 도입: 자바스크립트의 토큰 접근을 차단.

로그아웃 로직 강화: 서버 측에서 Refresh Token을 삭제하고, 클라이언트 쿠키 만료(max-age=0)를 동시에 수행.

3. 🐳 Docker 환경 변수 격리 및 배포

문제: 로컬(application.yml)과 Docker 컨테이너 환경 간 OAuth Redirect URI 불일치로 400 에러 발생.

해결: docker-compose.yml 환경 변수 주입을 통해 배포 환경에 맞는 Callback URI(.../link/callback)를 별도로 주입하여 해결.

📂 프로젝트 구조 (Directory Structure)

src/main/java/com/dialog
├── config          # Security, Web, Swagger 설정
├── domain          # Entity 및 DTO 정의
├── controller      # API 엔드포인트
├── service         # 비즈니스 로직
├── repository      # JPA Repository
├── security        # JWT, OAuth2, UserDetails 관련
└── global          # Global Exception Handler, Common Response


🔗 링크 (Links)

Notion Portfolio: https://www.notion.so/DialoG-STT-2bb357308ca280f297c6f86b0faba013?source=copy_link

Demo Video: [유튜브 링크 등이 있다면 여기에 넣으세요]

👨‍💻 Author

Name: 강승훈

Contact: [이메일 주소]

Role: Backend Lead, DevOps, Frontend Support

© 2025 Kang Seung Hoon. All Rights Reserved.
