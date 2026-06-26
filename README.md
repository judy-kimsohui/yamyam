# YamYamLog - 영상 기반 AI 식단 관리 서비스


### 🎏 배포 URL  [https://yamyamlog.site](https://yamyamlog.site) 
    (^^ゞ Sign in to Mock
<img width="1468" height="824" alt="image" src="https://github.com/user-attachments/assets/c67a9c66-43b6-464f-ac70-a9d7f9434ece" />




## 01. 프로젝트 개요

YamYamLog는 먹는 순간을 가볍게 기록하고, 기록을 습관과 성취로 연결하는 라이프로그 플랫폼입니다.

음식 영상을 업로드하는 것만으로 AI(GPT-4o)가 칼로리·탄수화물·단백질·지방을 자동 산출하고, 캘린더 기반 마이로그로 일별 섭취 현황을 시각화합니다. 그룹 기능으로 친구와 식단을 공유하고 실시간 채팅으로 소통할 수 있으며, 하루가 끝나면 AI가 개인 신체 정보를 바탕으로 맞춤 피드백을 생성합니다.

| 목표 | 설명 |
|------|------|
| NO BORING | 영상 업로드만으로 기록 완료 — 수기 입력 없이 AI가 자동 분석 |
| FANCY UI | 감각적인 비주얼과 TikTok 스타일 피드로 기록의 즐거움 제공 |
| SHARE WITH | 혼자 기록하는 앱을 넘어, 그룹과 함께 성장하는 커뮤니티 공간 |



## 02. 화면 미리보기

| 홈화면 | 회원가입 / 로그인 |
|:---:|:---:|
| ![타이틀](https://github.com/user-attachments/assets/1f121963-498c-433f-b0e6-948de9ad0f00) | ![회원가입/로그인](https://github.com/user-attachments/assets/f059cb4f-4b2e-420b-88fe-f6256f8c5ba7) |

| 메인 대시보드 (오늘의 로그 & 그룹 목록) | 그룹 초대 |
|:---:|:---:|
| ![메인 대시보드](https://github.com/user-attachments/assets/fc2661a3-e346-45f1-9ece-9e6929b67f7e) | ![그룹 초대](https://github.com/user-attachments/assets/fe3e72b7-3d1b-49e6-8eda-95825fae9971) |

| 그룹 대시보드 (그룹 로그 & AI 분석) | 마이 로그 & AI 종합 영양 평가 |
|:---:|:---:|
| ![그룹 대시보드](https://github.com/user-attachments/assets/207fcf27-30ed-44d0-acd2-4202b1a2200e) | ![AI 영양 정보](https://github.com/user-attachments/assets/dd7cbb7d-8a65-466a-b585-df0aec87a4be) |  


| AI 인식 음식 & 영양 정보 | AI 인식 정보 수정 / 저장 |
|:---:|:---:|
| ![모바일1](https://github.com/user-attachments/assets/7140660e-aa32-42e7-a92a-fdb7bf2e66db) | ![AI 정보 수정](https://github.com/user-attachments/assets/174d47b4-4f3c-4d4c-90d5-10d1c93c2e61) | 



| 모바일 뷰 (그룹 대시보드) | 모바일 뷰 (채팅) |
|:---:|:---:|
| ![모바일2](https://github.com/user-attachments/assets/79faeaff-be8a-4b4d-bb68-5c74910fe20e) | ![모바일3](https://github.com/user-attachments/assets/e8a891d2-6130-4605-ae7b-c583797bbad9) |



## 03. 핵심 기능

### 영상 업로드 & AI 영양 분석
- 음식 영상 업로드 → JavaCV로 대표 프레임 자동 추출
- 추출 이미지를 GPT-4o Vision API에 전송 → 식재료·영양 성분 자동 산출
- 분석 상태(`PENDING` → `DONE` / `FAILED`) 실시간 반영, 실패 시 최대 3회 재시도

### 마이로그 (캘린더 식단 기록)
- 날짜별 칼로리·3대 영양소를 달력에 표시
- 목표 대비 달성률, 매크로 진행 바, 음식 목록을 일별 리포트로 제공
- 신체 정보(나이·키·몸무게·목표 체중·성별)에 기반한 개인 목표 자동 산정

### AI 데일리 피드백
- 하루 식사 기록을 분석해 섭취 패턴 평가 및 개선 조언 생성
- 사용자 신체 정보와 목표 체중을 참고한 맞춤형 자연어 피드백 (2~3문장)

### 그룹(팀) 소셜 기능
- 팀 생성 후 초대 코드로 친구 초대 (URL 접근 시 자동 입장)
- TikTok 스타일의 그룹 피드에서 멤버 식단 영상 탐색 (GraphQL)
- WebSocket(STOMP) 기반 실시간 그룹 채팅, 메시지는 MongoDB에 영구 저장



## 04. 기술 스택

### Frontend

| 기술 | 버전 | 역할 |
|------|------|------|
| Vue.js | 3.5.x | UI 프레임워크 (SPA) |
| Vite | 8.0.x | 빌드 도구 |
| Axios | 1.7.x | HTTP 클라이언트 |
| STOMP.js | 7.x | WebSocket 실시간 채팅 |
| HLS.js | 1.6.x | 동영상 스트리밍 |
| SockJS | 1.6.x | WebSocket 폴백 |

### Backend

| 기술 | 버전 | 역할 |
|------|------|------|
| Java + Spring Boot | 21 / 3.5.x | 핵심 웹 프레임워크 |
| Spring WebFlux | — | 비동기 AI API 호출 |
| Spring WebSocket + STOMP | — | 실시간 채팅 |
| Spring GraphQL | — | 영상 피드 쿼리 |
| Spring AI | 1.0.0-M1 | Vision AI 연동 |
| MyBatis | 3.0.5 | 데이터 접근 계층 |
| MySQL 8 | — | 관계형 데이터 저장 |
| MongoDB | — | 채팅 메시지 저장 |
| JWT (jjwt) | 0.11.5 | 인증 토큰 |
| JavaCV (bytedeco) | 1.5.10 | 영상 프레임 추출 |
| AWS S3 SDK | 2.26.x | 영상 파일 저장 (운영) |

### 외부 서비스 / 인프라

| 항목 | 용도 |
|------|------|
| SSAFY GMS (OpenAI 호환 / GPT-4o) | 음식 인식 AI 분석, 데일리 피드백 생성 |
| AWS EC2 + S3 | 서버 호스팅 / 영상 업로드 · 스트리밍 |
| Nginx | 리버스 프록시 (80:5174) |
| Docker Compose | MySQL · MongoDB 컨테이너 관리 |
| CI/CD | 자동 빌드 · 배포 |



## 05. 시스템 아키텍처

### 인프라 구성

개발(DEV) 환경과 운영(PROD) 환경의 구성도입니다.

<img width="1468" height="751" alt="image" src="https://github.com/user-attachments/assets/8e9ffe83-a546-43c0-9c57-a5899b94001b" />


### AI 영상 분석 파이프라인

```
영상 업로드
    ↓
S3(운영) / 로컬(개발) 저장
    ↓
VideoFrameExtractor — JavaCV로 대표 프레임 추출
    ↓
Base64 인코딩 → GPT-4o Vision API 비동기 호출 (WebClient)
    ↓
응답 파싱 (Regex 전처리) → RecognizedFoodItem 목록 저장
    ↓
칼로리·탄수화물·단백질·지방 합산 → NutritionAnalysis 저장
    ↓
GET /api/videos/{videoId}/nutrition 으로 결과 조회
```

### 인증 흐름

```
로그인 (아이디 / 비밀번호)
    ↓
서버에서 JWT 발급
    ↓
클라이언트 메모리에 토큰 저장
    ↓
모든 요청: Authorization: Bearer <token> 헤더 전송
    ↓
JwtInterceptor 토큰 검증 → loginUserKey 설정
```



## 06. 트러블슈팅

| 문제 | 원인 | 해결 |
|------|------|------|
| 비동기 미디어 파이프라인 Race Condition | DB 트랜잭션 커밋 전에 `@Async` 스레드가 데이터 조회 시도 | `TransactionSynchronizationManager`로 커밋 완료 후 비동기 작업 트리거 |
| AI 응답 JSON 역직렬화 실패 | GPT-4o의 간헐적 환각(마크다운 백틱, 부연 설명 혼입) | 시스템 프롬프트에 Strict JSON 조건 강제 + 수신 후 Regex 기반 전처리 방어 로직 추가 |
| 피드 조회 N+1 쿼리 부하 | 영상·업로더·영양·좋아요 데이터 연관관계 파편화 | GraphQL 도입으로 필요 필드만 선택 조회 + JPA Fetch Join · Batch Size 최적화 |



## 07. 프로젝트 구조

```
yamyam/
├── yamyam-vue/                        # Vue.js 3 SPA
│   └── src/
│       ├── views/
│       │   ├── HomeView.vue           # 랜딩 / 로그인 · 회원가입
│       │   ├── MyLogView.vue          # 캘린더 식단 기록
│       │   ├── GroupsView.vue         # 그룹 목록 / 참여
│       │   ├── GroupDetailView.vue    # 그룹 피드
│       │   ├── ChatView.vue           # 실시간 그룹 채팅
│       │   └── DashboardView.vue      # 대시보드
│       ├── components/
│       │   ├── FeedCard.vue           # 영상 피드 카드
│       │   ├── VideoDetailModal.vue   # 영상 상세 / 영양 정보
│       │   ├── VideoUploadModal.vue   # 영상 업로드
│       │   └── BottomNav.vue          # 하단 네비게이션
│       └── composables/
│           ├── useStore.js            # 전역 상태 (그룹 · 사용자)
│           └── useToast.js            # 토스트 알림
│
└── yamyam-spring/                     # Spring Boot 3 REST API
    └── src/main/java/com/ssafy/yamyam/
        ├── domain/
        │   ├── user/                  # 회원 관리
        │   ├── video/                 # 영상 업로드 · 조회 · GraphQL
        │   ├── nutrition/             # AI 분석 · 영양 조회 · 데일리 피드백
        │   ├── team/                  # 그룹 생성 · 참여 · 초대
        │   └── chat/                  # WebSocket 채팅
        ├── config/
        │   ├── WebSocketConfig.java
        │   ├── OpenAiConfig.java
        │   ├── S3Config.java
        │   └── AsyncConfig.java
        └── global/
            ├── interceptor/JwtInterceptor.java
            └── util/JwtUtil.java
```



## 08. API 엔드포인트

| 그룹 | 경로 | 주요 기능 |
|------|------|-----------|
| 회원 | `/api/users/*` | 회원가입 · 로그인 · 프로필 조회 |
| 영상 | `/api/videos/*` | 업로드 · 조회 · 삭제 · Presigned URL |
| 영양 분석 | `/api/videos/{id}/nutrition` | AI 분석 결과 조회 |
| 분석 트리거 | `/api/videos/{id}/analyze` | 분석 시작 · 재시도 |
| 식단 로그 | `/api/logs/*` | 일별 기록 조회 · AI 피드백 |
| 팀 | `/api/teams/*` | 그룹 생성 · 목록 · 참여 · 초대 코드 |
| 채팅 | WebSocket `/ws/chat` | 실시간 그룹 채팅 (STOMP) |
| 피드 | GraphQL `/graphql` | 영상 피드 목록 조회 |



## 09. 실행 방법

### 사전 요구사항

- Java 21+, Node.js 20+, MySQL 8.0+, MongoDB

### 환경 변수 설정

```bash
# 데이터베이스
SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/yamyam
SPRING_DATASOURCE_USERNAME=<db_user>
SPRING_DATASOURCE_PASSWORD=<db_password>
SPRING_DATA_MONGODB_URI=mongodb://localhost:27017/yamyam

# 인증
JWT_SECRET=<jwt_secret_key>

# AI (SSAFY GMS) — 미설정 시 AI 기능만 비활성화, 나머지 정상 동작
SPRING_AI_OPENAI_BASE_URL=https://gms.ssafy.io/gmsapi
SPRING_AI_OPENAI_API_KEY=<gms_api_key>

# AWS S3 (운영 환경만)
AWS_S3_BUCKET=<bucket_name>
AWS_REGION=<region>
```

### 실행

```bash
# Backend (기본 포트: 8080)
cd yamyam-spring
./mvnw spring-boot:run

# Frontend (기본 포트: 5173)
cd yamyam-vue
npm install
npm run dev
```

### 빌드

```bash
cd yamyam-vue && npm run build
cd yamyam-spring && ./mvnw package
```



## 팀원

| <img src="https://github.com/judy-kimsohui.png" width="100" height="100" style="border-radius:50%"/> | <img src="https://github.com/gwamul.png" width="100" height="100" style="border-radius:50%"/> |
|:---:|:---:|
| **김소희** | **정광석** |
| [@judy-kimsohui](https://github.com/judy-kimsohui) | [@gwamul](https://github.com/gwamul) |
| 풀스택 | 풀스택 |
| 영상 업로드 · 데이터 처리 · 비동기 파이프라인 | AI 연동 · 화면 상태 동기화 · UI 최적화 |
| 영상 처리 성능 저하와 서버 부하 문제를 비동기 구조 개선으로 해결하며 안정적인 서비스 운영 역량을 키웠습니다. 다음 프로젝트에서는 초기 설계 단계부터 팀 간 API 명세와 인터페이스 정의를 강화하고 싶습니다. | GPT-4o 기반 영양 분석 자동화로 사용자 편의를 극대화하고, 트랜잭션 동기화를 통한 비동기 처리로 백엔드 시스템 안정성까지 경험할 수 있던 프로젝트였습니다. |
