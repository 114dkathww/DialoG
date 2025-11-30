# 📅 개발 업무 일지 (Development Log)

> **프로젝트 진행 기간:** 2025.09.29 ~ 2025.11.28 (8주)
> **작성자:** Team Lead (PM) & Backend Developer
> **개요:** 프로젝트 기획부터 배포까지의 주간 업무 진행 상황과 주요 트러블 슈팅 내역을 기록한 문서입니다.

---

## 1. 프로젝트 착수 및 설계 (09.29 ~ 10.03)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **기획:** 기능 명세서 작성 및 WBS 일정 수립<br>• **DB:** 초기 ERD 설계 및 정규화, 불필요한 테이블 제거<br>• **UI:** 프론트엔드 프로토타입(로그인, 회원가입, 캘린더) 개발<br>• **환경:** GitHub 리포지토리 생성 및 팀 컨벤션 확립 |
| **특이 사항** | • IT 용어 및 기술 스택(Python/Java 상호운용성) 사전 조사 수행 |

## 2. 보안 시스템 구축 및 초기 연동 (10.13 ~ 10.17)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **Security:** Spring Security 연동 및 JWT 토큰 발급/검증 로직 구현<br>• **Frontend:** JS Fetch API와 백엔드 API 통신 연결<br>• **AI:** CLOVA ChatBot FAQ 데이터 학습 및 도메인 생성 |
| **이슈 해결** | • **Docker 연동:** Spring Boot와 MySQL Docker 컨테이너 간 네트워크 연결 테스트<br>• **ERD 동기화:** JPA Entity와 실제 DB 테이블 간의 매핑 불일치 수정 |

## 3. 소셜 로그인 및 외부 API (10.17 ~ 10.23)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **Auth:** Google/Kakao 소셜 로그인 구현 및 Refresh Token DB 설계<br>• **API:** Google 캘린더 API 권한 처리 및 이벤트 연동<br>• **STT:** CLOVA Speech gRPC 스트리밍 서버(FastAPI) 구축 |
| **Trouble Shooting** | • **Google Login Error:** OIDC 방식 사용 시 `loadUser` 미호출 문제 발생 <br>→ `application.yml` 설정을 변경하여 `CustomOAuth2UserService`를 타도록 수정.<br>• **WebSocket Timing:** STT 분석 완료 메시지(`done`)가 파일 업로드보다 먼저 도착하는 이슈 해결 |

## 4. 핵심 기능 고도화 (10.24 ~ 10.30)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **Meeting:** 회의/참여자/키워드 엔티티 관계 매핑 및 생성 로직 구현<br>• **Dashboard:** 홈 화면에 사용자 정보(이름, 이메일) 및 To-Do 연동<br>• **STT:** 화자 분리(Diarization) 및 장문 인식 로직 개선 |
| **Trouble Shooting** | • **FK 제약조건:** 회의 삭제 시 연관 데이터(참여자)로 인한 삭제 불가 확인<br>→ 삭제 순서를 보장하는 서비스 로직 구현.<br>• **PyAudio 호환성:** Docker Linux 환경에서 PyAudio 빌드 실패<br>→ 브라우저 `WebAudio` API + PCM Chunk 전송 방식으로 아키텍처 변경 |

## 5. 관리자 시스템 및 토큰 재발급 (10.31 ~ 11.06)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **Auth:** JWT Access Token 재발급 및 Silent Refresh 구현<br>• **Storage:** Naver Object Storage 연동 (녹음 파일 업로드/다운로드)<br>• **Admin:** 관리자 권한(RBAC) 분리 및 전용 대시보드 UI 기획 |
| **이슈 해결** | • **STT 뷰 누락:** 음성 변환 텍스트가 프론트엔드 뷰에 렌더링되지 않는 문제<br>→ JS 비동기 처리 로직 수정으로 발화점 구분 및 출력 정상화 |

## 6. 데이터 시각화 및 예외 처리 (11.07 ~ 11.13)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **Dashboard:** 관리자 통계(전일 대비 증감률) API 구현 및 차트 시각화<br>• **Exception:** Global Exception Handler 도입 및 커스텀 예외 클래스 정의<br>• **Chatbot:** 회의록 기반 RAG(검색 증강 생성) 로직 통합 |
| **Trouble Shooting** | • **400/500 에러 핸들링:** 구글 캘린더 토큰 만료 시 발생하는 에러를 커스텀 예외로 래핑하여 처리.<br>• **JSON 순환 참조:** 양방향 연관관계 엔티티 조회 시 무한 루프 발생 → DTO 변환 로직으로 해결 |

## 7. 편의성 개선 및 안정화 (11.14 ~ 11.20)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **User:** 비밀번호 찾기(SMTP 이메일 발송) 및 재설정 프로세스 구현<br>• **UX:** 회원가입 약관 동의 분리 및 직무 미설정 유저 온보딩 모달<br>• **Refactoring:** `recordFinish` 페이지 코드 분리 및 API 호출 최적화 |
| **Trouble Shooting** | • **Zombie Data:** 캘린더 삭제 일정이 DB 동기화 시 부활하는 버그<br>→ API 응답의 `status: cancelled` 필드 필터링 로직 추가.<br>• **Docker Env:** 로컬과 Docker 컨테이너 간 환경 변수 불일치로 인한 OAuth 오류 해결 |

## 8. 최종 배포 및 UI 폴리싱 (11.21 ~ 11.28)
| 구분 | 상세 내용 |
| :--- | :--- |
| **주요 업무** | • **DevOps:** Docker Compose 배포 환경 구축 및 프록시 설정<br>• **Optimization:** 캘린더 데이터 정합성 확보 및 쿼리 튜닝<br>• **Final:** 최종 UI/UX 테스트 및 시연 준비 (PPT, 영상) |
| **이슈 해결** | • **데이터 중복:** 새 회의 생성 시 캘린더에 2개씩 렌더링되는 현상 수정.<br>• **트랜잭션:** 중요도 표시 변경 시 롤백 문제 → 명시적 `save` 호출로 해결 |

---
*이 문서는 프로젝트 기간 동안 작성된 주간 업무 보고서를 기반으로 정리되었습니다.*
