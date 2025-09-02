# RealWorld MVP 구현 작업 목록

본 문서는 교육/테스트용 MVP 수준의 RealWorld 구현을 위한 간소화된 작업 목록입니다.

## Phase 1: 기본 환경 구성

### 1.1 프로젝트 구조
- [ ] 백엔드 Go 프로젝트 생성 (`/backend`)
- [ ] 프론트엔드 React + Vite 프로젝트 생성 (`/frontend`)
- [ ] 기본 README.md 작성

### 1.2 백엔드 기본 설정
- [ ] Go 모듈 초기화
- [ ] Gin HTTP 서버 설정
- [ ] SQLite 데이터베이스 연결
- [ ] 기본 미들웨어 (CORS, 로깅)
- [ ] 환경변수 설정

### 1.3 프론트엔드 기본 설정
- [ ] React + Vite 프로젝트 초기화
- [ ] Tailwind CSS 설정
- [ ] React Router 설정
- [ ] 기본 레이아웃 컴포넌트

## Phase 2: 핵심 기능 구현

### 2.1 데이터베이스 스키마
- [ ] 사용자 테이블 (users)
- [ ] 게시글 테이블 (articles)
- [ ] 댓글 테이블 (comments)
- [ ] 기본 시드 데이터

### 2.2 사용자 인증 시스템

#### 백엔드
- [ ] JWT 토큰 유틸리티
- [ ] 비밀번호 해싱 (bcrypt)
- [ ] `POST /api/users/login` (로그인)
- [ ] `POST /api/users` (회원가입)
- [ ] `GET /api/user` (현재 사용자)
- [ ] 인증 미들웨어

#### 프론트엔드
- [ ] 로그인 페이지
- [ ] 회원가입 페이지
- [ ] 인증 Context
- [ ] 토큰 로컬 스토리지 관리

### 2.3 게시글 시스템

#### 백엔드
- [ ] `GET /api/articles` (게시글 목록)
- [ ] `GET /api/articles/:slug` (게시글 상세)
- [ ] `POST /api/articles` (게시글 생성)
- [ ] `PUT /api/articles/:slug` (게시글 수정)
- [ ] `DELETE /api/articles/:slug` (게시글 삭제)

#### 프론트엔드
- [ ] 홈 페이지 (게시글 목록)
- [ ] 게시글 상세 페이지
- [ ] 게시글 작성/수정 페이지
- [ ] 간단한 Markdown 렌더링

### 2.4 댓글 시스템

#### 백엔드
- [ ] `GET /api/articles/:slug/comments` (댓글 목록)
- [ ] `POST /api/articles/:slug/comments` (댓글 작성)
- [ ] `DELETE /api/articles/:slug/comments/:id` (댓글 삭제)

#### 프론트엔드
- [ ] 댓글 목록 컴포넌트
- [ ] 댓글 작성 폼
- [ ] 댓글 삭제 기능

## Phase 3: 추가 기능 (선택사항)

### 3.1 사용자 프로필
- [ ] `GET /api/profiles/:username` (프로필 조회)
- [ ] 프로필 페이지

### 3.2 태그 시스템
- [ ] `GET /api/tags` (태그 목록)
- [ ] 태그별 필터링 (간단한 버전)

### 3.3 좋아요 기능
- [ ] `POST /api/articles/:slug/favorite` (좋아요)
- [ ] `DELETE /api/articles/:slug/favorite` (좋아요 취소)
- [ ] 좋아요 버튼

## Phase 4: 배포 준비

### 4.1 Docker 설정
- [ ] 백엔드 Dockerfile
- [ ] 프론트엔드 Dockerfile
- [ ] docker-compose.yml

### 4.2 기본 에러 처리
- [ ] API 에러 응답 표준화
- [ ] 프론트엔드 에러 바운더리
- [ ] 로딩 상태 처리

## 최소 기능 요구사항

### 필수 기능 (MVP)
- [x] 사용자 회원가입/로그인
- [x] 게시글 작성/조회/수정/삭제
- [x] 댓글 작성/조회/삭제
- [x] 반응형 기본 UI

### 선택 기능
- [ ] 사용자 프로필
- [ ] 태그 시스템
- [ ] 좋아요 기능
- [ ] 팔로우 시스템

## 기술 스택 간소화

### 백엔드
- **언어**: Go
- **프레임워크**: Gin
- **데이터베이스**: SQLite (파일 기반)
- **인증**: JWT

### 프론트엔드
- **프레임워크**: React 18
- **빌드 도구**: Vite
- **스타일링**: Tailwind CSS
- **라우팅**: React Router
- **HTTP 클라이언트**: Fetch API

### 배포
- **컨테이너**: Docker
- **개발 환경**: docker-compose

## 성공 기준

1. **기본 사용자 플로우 완성**
   - 회원가입 → 로그인 → 게시글 작성 → 댓글 작성

2. **API 호환성**
   - RealWorld API 기본 엔드포인트 동작

3. **UI 기본 기능**
   - 모바일/데스크톱 반응형
   - 인증 상태에 따른 UI 변화

## 예상 개발 시간: 2-3주

이 MVP는 교육용으로 핵심 기능에 집중하여 RealWorld의 기본 구조를 이해하고 학습하는데 최적화되었습니다.