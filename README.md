# YamYamLog - 영상 기반 AI 식단 관리 서비스

## 🌐 배포 URL

**https://yamyamlog.site**

---

## 프로젝트 소개

먹은 음식을 짧은 영상으로 기록하면, AI가 영상 프레임을 분석해 **식재료와 영양 정보를 자동으로 추출**해 주는 식단 관리 플랫폼입니다.

칼로리·탄수화물·단백질·지방 목표를 직접 설정하고, 캘린더 기반의 마이로그로 일별 섭취 현황을 한눈에 확인할 수 있습니다.
그룹(팀)을 만들어 초대 코드로 친구와 함께 식단을 공유하고, 그룹 채팅으로 소통하는 소셜 기능도 제공합니다.
하루 식사를 마치면 AI가 개인 정보(나이·체중·목표 체중)를 바탕으로 **맞춤형 데일리 식단 피드백**을 생성합니다.

프론트엔드는 **Vue.js 3 SPA** 구조로, 백엔드는 **Spring Boot 3 + MyBatis MVC** 아키텍처로 구현되었습니다.

---

## 팀원

| 이름 | 역할 |
|------|------|
| 김소희 | 풀스택 개발 |
| 정광석 | 풀스택 개발 |

---

## 기술 스택

### Frontend

| 기술 | 버전 | 용도 |
|------|------|------|
| Vue.js | 3.5.x | UI 프레임워크 |
| Vite | 8.0.x | 빌드 도구 |
| Axios | 1.7.x | HTTP 클라이언트 |
| STOMP.js | 7.x | WebSocket 실시간 채팅 |
| HLS.js | 1.6.x | 동영상 스트리밍 |
| SockJS | 1.6.x | WebSocket 폴백 |

### Backend

| 기술 | 버전 | 용도 |
|------|------|------|
| Java | 21 | 프로그래밍 언어 |
| Spring Boot | 3.5.x | 웹 프레임워크 |
| Spring WebFlux | - | 비동기 AI API 호출 |
| Spring WebSocket + STOMP | - | 실시간 채팅 |
| Spring GraphQL | - | 영상 피드 쿼리 |
| Spring AI | 1.0.0-M1 | AI 연동 |
| MyBatis | 3.0.5 | 데이터 접근 계층 |
| MySQL | 8 | 관계형 데이터베이스 |
| MongoDB | - | 채팅 메시지 저장 |
| JWT (jjwt) | 0.11.5 | 인증 토큰 |
| JavaCV (bytedeco) | 1.5.10 | 영상 프레임 추출 |
| AWS S3 SDK | 2.26.x | 영상 파일 저장 (prod) |
| Maven | - | 빌드 도구 |

### 외부 API / 인프라

| 항목 | 용도 |
|------|------|
| SSAFY GMS (OpenAI 호환) | 음식 인식 AI 분석, 데일리 식단 피드백 |
| AWS S3 | 운영 환경 영상 업로드 / 스트리밍 |

---

## 주요 기능

### 영상 업로드 & AI 영양 분석

- 음식 영상을 업로드하면 JavaCV로 주요 프레임을 자동 추출
- 추출된 이미지를 AI(SSAFY GMS)에 전송해 식재료와 영양 성분(칼로리·탄수화물·단백질·지방) 자동 산출
- 분석이 완료되면 영상 카드에 영양 정보 표시

### 마이로그 (캘린더 식단 기록)

- 날짜별 섭취 칼로리·3대 영양소를 달력에 표시
- 일별 영양 리포트: 목표 대비 달성률, 매크로 진행 바, 음식 목록 확인
- 회원 프로필(나이·키·몸무게·목표 체중·성별)에 기반한 개인 목표 자동 산정

### AI 데일리 피드백

- 하루 식사 기록을 바탕으로 AI가 섭취 패턴을 평가하고 개선 조언 제공
- 사용자 신체 정보와 목표 체중을 참고한 맞춤 피드백 생성

### 그룹(팀) 기능

- 팀 생성 후 초대 코드 공유로 친구 초대
- 여러 그룹 참여 가능, 그룹별 피드 분리 제공
- 초대 코드로 URL 직접 접근 시 자동 입장 처리

### 실시간 그룹 채팅 (WebSocket / STOMP)

- 그룹원끼리 실시간 채팅 지원
- 채팅 메시지 MongoDB 영구 저장
- 입장·퇴장 시스템 메시지 자동 전송

### 소셜 피드

- 그룹 내 다른 멤버의 식단 영상 피드를 TikTok 스타일로 탐색
- GraphQL을 통한 영상 목록 조회

### 회원 관리

- 아이디·비밀번호 기반 회원가입 / 로그인
- 나이·성별·키·현재 체중·목표 체중 등 신체 정보 등록
- JWT 기반 인증 (Authorization 헤더)

---

## 인증 흐름

1. 아이디·비밀번호로 로그인 → JWT 발급
2. 클라이언트 메모리에 토큰 저장, 이후 요청마다 `Authorization: Bearer <token>` 헤더 전송
3. 서버 인터셉터(`JwtInterceptor`)가 토큰 검증 후 `loginUserKey` 속성 설정

---

## AI 영상 분석 파이프라인

```
영상 업로드
    ↓
S3(prod) / 로컬(dev) 저장
    ↓
VideoFrameExtractor — JavaCV로 대표 프레임(이미지) 추출
    ↓
Base64 인코딩 후 SSAFY GMS Vision API 호출 (비동기)
    ↓
응답 파싱 → RecognizedFoodItem 목록 저장
    ↓
칼로리·탄수화물·단백질·지방 합산 → NutritionAnalysis 저장
    ↓
프론트에서 /api/videos/{videoId}/nutrition 으로 결과 조회
```

- 분석 상태: `PENDING` → `DONE` / `FAILED`
- `FAILED` 시 최대 3회까지 재분석 가능 (`/api/videos/{videoId}/analyze`)
- GMS API 키 미설정 시 AI 기능만 비활성화, 나머지 기능은 정상 동작

---

## 프로젝트 구조

```
yamyam/
├── yamyam-vue/                   # Vue.js 3 SPA
│   └── src/
│       ├── views/
│       │   ├── HomeView.vue      # 랜딩 / 로그인·회원가입
│       │   ├── MyLogView.vue     # 캘린더 식단 기록
│       │   ├── GroupsView.vue    # 그룹 목록 / 참여
│       │   ├── GroupDetailView.vue # 그룹 피드
│       │   ├── ChatView.vue      # 실시간 그룹 채팅
│       │   └── DashboardView.vue # 대시보드
│       ├── components/
│       │   ├── FeedCard.vue      # 영상 피드 카드
│       │   ├── StickerVideo.vue  # 영상 + 스티커 오버레이
│       │   ├── VideoDetailModal.vue  # 영상 상세 / 영양 정보
│       │   ├── VideoUploadModal.vue  # 영상 업로드
│       │   ├── LoginModal.vue    # 로그인 폼
│       │   ├── SignupModal.vue   # 회원가입 폼
│       │   └── BottomNav.vue     # 하단 네비게이션
│       └── composables/
│           ├── useStore.js       # 전역 상태 (그룹·사용자)
│           └── useToast.js       # 토스트 알림
│
└── yamyam-spring/                # Spring Boot 3 REST API
    └── src/main/java/com/ssafy/yamyam/
        ├── domain/
        │   ├── user/             # 회원 관리 (UserController, UserService)
        │   ├── video/            # 영상 업로드·조회·GraphQL (VideoController, VideoGraphQLController)
        │   ├── nutrition/        # AI 분석·영양 조회·로그·데일리 피드백
        │   │   ├── controller/   # NutritionController, LogController
        │   │   └── service/      # VideoAnalysisService, DailyAiService, VideoFrameExtractor
        │   ├── team/             # 그룹 생성·참여·초대 (TeamController)
        │   └── chat/             # WebSocket 채팅 (ChatController)
        ├── config/
        │   ├── WebSocketConfig.java
        │   ├── OpenAiConfig.java
        │   ├── S3Config.java
        │   └── AsyncConfig.java
        └── global/
            ├── interceptor/JwtInterceptor.java
            └── util/JwtUtil.java
```

---

## 페이지 라우팅

| 화면 | 설명 | 인증 필요 |
|------|------|-----------|
| `/` (HomeView) | 랜딩·로그인·회원가입 | - |
| `groups` | 내 그룹 목록 / 참여 | 필요 |
| `group-detail` | 그룹 피드 (영상 목록) | 필요 |
| `calendar` | 마이로그 (캘린더·영양 리포트) | 필요 |
| `chat` | 실시간 그룹 채팅 | 필요 |
| `dashboard` | 대시보드 (팀·프로필) | 필요 |

---

## 실행 방법

### 사전 요구사항

- Java 21+
- Node.js 20+ (또는 22+)
- MySQL 8.0+
- MongoDB

### 환경 변수 설정

백엔드 실행 전 아래 환경 변수를 설정하세요.

```bash
# 데이터베이스
SPRING_DATASOURCE_URL=jdbc:mysql://localhost:3306/yamyam
SPRING_DATASOURCE_USERNAME=<db_user>
SPRING_DATASOURCE_PASSWORD=<db_password>

SPRING_DATA_MONGODB_URI=mongodb://localhost:27017/yamyam

# JWT
JWT_SECRET=<jwt_secret_key>

# AI (SSAFY GMS)
SPRING_AI_OPENAI_BASE_URL=https://gms.ssafy.io/gmsapi
SPRING_AI_OPENAI_API_KEY=<gms_api_key>   # 미설정 시 AI 기능만 비활성화

# AWS S3 (prod 환경만)
AWS_S3_BUCKET=<bucket_name>
AWS_REGION=<region>
```

### Backend 실행

```bash
cd yamyam-spring
./mvnw spring-boot:run
```

> 기본 포트: `http://localhost:8080`

### Frontend 실행

```bash
cd yamyam-vue
npm install
npm run dev
```

> 기본 포트: `http://localhost:5173`

### 빌드

```bash
# Frontend
cd yamyam-vue
npm run build

# Backend
cd yamyam-spring
./mvnw package
```

---

## API 엔드포인트 개요

| 그룹 | 경로 | 주요 기능 |
|------|------|-----------|
| 회원 | `/api/users/*` | 회원가입·로그인·프로필 조회 |
| 영상 | `/api/videos/*` | 업로드·조회·삭제·Presigned URL |
| 영양 분석 | `/api/videos/{id}/nutrition` | AI 분석 결과 조회 |
| AI 분석 트리거 | `/api/videos/{id}/analyze` | 분석 시작·재시도 |
| 로그 | `/api/logs/*` | 일별 식단 기록 조회·AI 피드백 |
| 팀 | `/api/teams/*` | 그룹 생성·목록·참여·초대 코드 |
| 채팅 | WebSocket `/ws/chat` | 실시간 그룹 채팅 |
| 피드 | GraphQL `/graphql` | 영상 피드 목록 조회 |

---

## 프로젝트 목표

- **Vue.js 3**를 활용하여 SPA 웹 서비스를 구축할 수 있다.
- **Spring Boot 3 MVC + WebFlux** 혼합 아키텍처로 동기·비동기 처리를 적절히 활용할 수 있다.
- **JavaCV**를 이용한 서버 사이드 영상 처리 파이프라인을 구현할 수 있다.
- **Spring AI**를 활용하여 Vision 기반 음식 인식 서비스를 연동할 수 있다.
- **WebSocket(STOMP)** 을 이용한 실시간 통신 기능을 구현할 수 있다.
- **AWS S3**를 활용한 대용량 영상 파일 저장·스트리밍 흐름을 설계할 수 있다.
- **MySQL + MongoDB** 이중 저장소를 목적에 맞게 분리하여 운용할 수 있다.
