# Spotify Insights

Spotify 서드파티 프로그램 - 개인 청취 습관 심층 분석, 스마트 자동 플레이리스트 생성, 음악 추천 매칭 서비스

## 프로젝트 구조

```
.
├── apps/
│   ├── backend/          # NestJS Backend (DDD Architecture)
│   │   └── src/
│   │       ├── user/                    # 사용자 도메인
│   │       │   ├── domain/              # 엔티티, 값 객체, 도메인 서비스
│   │       │   ├── application/         # 유스케이스, 애플리케이션 서비스
│   │       │   ├── infrastructure/      # 레포지토리 구현
│   │       │   └── presentation/        # 컨트롤러, DTO
│   │       ├── listening-habits/        # 청취 습관 분석 도메인
│   │       │   ├── domain/
│   │       │   ├── application/
│   │       │   ├── infrastructure/
│   │       │   └── presentation/
│   │       ├── playlist/                # 플레이리스트 도메인
│   │       │   ├── domain/
│   │       │   ├── application/
│   │       │   ├── infrastructure/
│   │       │   └── presentation/
│   │       ├── recommendation/          # 추천 도메인
│   │       │   ├── domain/
│   │       │   ├── application/
│   │       │   ├── infrastructure/
│   │       │   └── presentation/
│   │       ├── track/                   # 트랙 도메인
│   │       │   ├── domain/
│   │       │   ├── application/
│   │       │   ├── infrastructure/
│   │       │   └── presentation/
│   │       ├── shared/                  # 공유 유틸리티
│   │       │   ├── guards/
│   │       │   ├── decorators/
│   │       │   ├── filters/
│   │       │   ├── interceptors/
│   │       │   ├── pipes/
│   │       │   └── utils/
│   │       ├── infrastructure/          # 공통 인프라
│   │       │   ├── database/
│   │       │   ├── spotify/
│   │       │   ├── cache/
│   │       │   └── auth/
│   │       ├── config/
│   │       ├── app.module.ts
│   │       └── main.ts
│   │
│   └── frontend/         # Next.js Frontend
│       └── src/
│           ├── app/                     # Next.js App Router
│           ├── components/              # React 컴포넌트
│           ├── lib/                     # 라이브러리 설정
│           ├── hooks/                   # Custom React Hooks
│           ├── services/                # API 서비스
│           ├── types/                   # 타입 정의
│           └── styles/                  # 스타일 파일
│
├── packages/
│   ├── types/            # 공유 TypeScript 타입
│   └── shared-utils/     # 공유 유틸리티 함수
│
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
└── tsconfig.json
```

## 기술 스택

### Backend
- **Framework**: NestJS
- **Architecture**: Domain-Driven Design (DDD)
- **Database**: PostgreSQL + TypeORM
- **Cache**: Redis
- **Authentication**: JWT + Passport (Spotify OAuth)
- **API Documentation**: Swagger

### Frontend
- **Framework**: Next.js 14 (App Router)
- **State Management**: Zustand
- **Data Fetching**: TanStack Query (React Query)
- **Styling**: Tailwind CSS
- **Charts**: Recharts

### Monorepo
- **Package Manager**: pnpm
- **Build System**: Turbo

## 주요 기능

### 1. 개인 청취 습관 심층 분석
- 시간대별/요일별 청취 패턴 분석
- 장르/아티스트 선호도 분석
- 음악 특성(템포, 에너지 등) 분석
- 청취 히스토리 시각화

### 2. 스마트 자동 플레이리스트 생성
- 사용자 청취 패턴 기반 자동 플레이리스트
- 무드/장르/시간대별 맞춤 플레이리스트
- 자동 업데이트 기능

### 3. 음악 추천 매칭 서비스
- AI 기반 개인화 추천
- 유사 취향 사용자 매칭
- 새로운 아티스트/장르 발견

## 시작하기

### 사전 요구사항

- Node.js >= 18
- pnpm >= 8
- PostgreSQL
- Redis
- Spotify Developer Account

### 환경 설정

1. 의존성 설치
```bash
pnpm install
```

2. 환경 변수 설정
```bash
# Backend
cp apps/backend/.env.example apps/backend/.env

# Frontend
cp apps/frontend/.env.example apps/frontend/.env
```

3. Spotify API 설정
- [Spotify Developer Dashboard](https://developer.spotify.com/dashboard)에서 앱 생성
- Client ID, Client Secret 발급
- Redirect URI 설정: `http://localhost:3001/api/v1/auth/spotify/callback`

### 개발 서버 실행

```bash
# 전체 실행
pnpm dev

# Backend만 실행
pnpm backend:dev

# Frontend만 실행
pnpm frontend:dev
```

### 빌드

```bash
pnpm build
```

## API 문서

개발 서버 실행 후 다음 주소에서 API 문서 확인 가능:
- Swagger UI: http://localhost:3001/api/docs

## 라이선스

MIT
