# 🎨 Vibe Coding - RealWorld 설계 문서

## 📋 개요

본 문서는 바이브 코딩 실습을 위한 RealWorld (Conduit) 애플리케이션의 시스템 설계를 정의합니다. Medium.com 클론 형태의 소셜 블로깅 플랫폼으로, Go 백엔드와 React 프론트엔드를 사용하는 교육용 MVP 구현을 목표로 합니다.

## 🎯 설계 목표

### 주요 목표
- **📚 교육 중심**: 학습하기 쉬운 명확한 구조
- **🔧 단순성 우선**: 복잡한 아키텍처보다 이해하기 쉬운 설계
- **⚡ 빠른 개발**: MVP 수준의 빠른 프로토타이핑
- **🔗 API 호환성**: RealWorld API 명세 준수

### 비기능적 요구사항
- **성능**: API 응답 시간 < 1초, 페이지 로딩 < 3초
- **확장성**: 동시 사용자 100명 지원
- **보안**: JWT 인증, XSS/SQL Injection 방지
- **호환성**: 모던 브라우저 지원, 반응형 디자인

## 🏗️ 시스템 아키텍처

### 전체 아키텍처 개요

```mermaid
graph TB
    subgraph "클라이언트"
        Browser[브라우저]
        Mobile[모바일 브라우저]
    end

    subgraph "프론트엔드 계층"
        React[React SPA]
        Router[React Router]
        UI[Tailwind CSS]
        State[Context API]
    end

    subgraph "백엔드 계층"
        API[Gin REST API]
        Auth[JWT 미들웨어]
        Handler[핸들러]
        Service[서비스 로직]
    end

    subgraph "데이터 계층"
        SQLite[(SQLite DB)]
        FileSystem[파일 시스템]
    end

    Browser --> React
    Mobile --> React
    React --> Router
    React --> UI
    React --> State
    React -->|HTTP/JSON| API
    API --> Auth
    API --> Handler
    Handler --> Service
    Service --> SQLite
    Service --> FileSystem
```

### 백엔드 아키텍처

```mermaid
graph TB
    subgraph "HTTP 계층"
        Router[Gin Router]
        CORS[CORS 미들웨어]
        Logger[로깅 미들웨어]
        Auth[JWT 인증 미들웨어]
    end

    subgraph "핸들러 계층"
        UserHandler[User Handler]
        ArticleHandler[Article Handler]
        CommentHandler[Comment Handler]
        ProfileHandler[Profile Handler]
    end

    subgraph "서비스 계층"
        UserService[User Service]
        ArticleService[Article Service]
        CommentService[Comment Service]
        AuthService[Auth Service]
    end

    subgraph "데이터 계층"
        UserRepo[User Repository]
        ArticleRepo[Article Repository]
        CommentRepo[Comment Repository]
        Database[(SQLite)]
    end

    Router --> CORS
    CORS --> Logger
    Logger --> Auth
    Auth --> UserHandler
    Auth --> ArticleHandler
    Auth --> CommentHandler
    Auth --> ProfileHandler

    UserHandler --> UserService
    ArticleHandler --> ArticleService
    CommentHandler --> CommentService
    ProfileHandler --> UserService

    UserService --> UserRepo
    ArticleService --> ArticleRepo
    CommentService --> CommentRepo
    AuthService --> UserRepo

    UserRepo --> Database
    ArticleRepo --> Database
    CommentRepo --> Database
```

### 프론트엔드 아키텍처

```mermaid
graph TB
    subgraph "라우팅 계층"
        Router[React Router]
        ProtectedRoute[인증 보호 라우트]
    end

    subgraph "페이지 계층"
        HomePage[홈 페이지]
        LoginPage[로그인 페이지]
        ArticlePage[게시글 페이지]
        EditorPage[에디터 페이지]
        ProfilePage[프로필 페이지]
    end

    subgraph "컴포넌트 계층"
        Layout[레이아웃]
        Header[헤더]
        ArticleList[게시글 목록]
        CommentList[댓글 목록]
        Forms[폼 컴포넌트]
    end

    subgraph "상태 관리"
        AuthContext[인증 Context]
        UserContext[사용자 Context]
        LocalStorage[Local Storage]
    end

    subgraph "API 계층"
        ApiClient[API 클라이언트]
        AuthService[인증 서비스]
        ArticleService[게시글 서비스]
    end

    Router --> ProtectedRoute
    ProtectedRoute --> HomePage
    ProtectedRoute --> ArticlePage
    ProtectedRoute --> EditorPage
    ProtectedRoute --> ProfilePage
    Router --> LoginPage

    HomePage --> Layout
    ArticlePage --> Layout
    EditorPage --> Layout
    
    Layout --> Header
    Layout --> ArticleList
    Layout --> CommentList
    Layout --> Forms

    AuthContext --> LocalStorage
    UserContext --> AuthContext
    
    HomePage --> ApiClient
    ArticlePage --> ApiClient
    EditorPage --> ApiClient
    
    ApiClient --> AuthService
    ApiClient --> ArticleService
```

## 💾 데이터베이스 설계

### ERD (Entity Relationship Diagram)

```mermaid
erDiagram
    users {
        int id PK
        string email UK
        string username UK
        string password_hash
        string bio
        string image
        datetime created_at
        datetime updated_at
    }

    articles {
        int id PK
        string slug UK
        string title
        string description
        string body
        int author_id FK
        datetime created_at
        datetime updated_at
    }

    comments {
        int id PK
        string body
        int article_id FK
        int author_id FK
        datetime created_at
        datetime updated_at
    }

    tags {
        int id PK
        string name UK
        datetime created_at
    }

    article_tags {
        int article_id FK
        int tag_id FK
    }

    favorites {
        int user_id FK
        int article_id FK
        datetime created_at
    }

    follows {
        int follower_id FK
        int followee_id FK
        datetime created_at
    }

    users ||--o{ articles : "작성"
    users ||--o{ comments : "작성"
    users ||--o{ favorites : "좋아요"
    users ||--o{ follows : "팔로워"
    users ||--o{ follows : "팔로잉"
    
    articles ||--o{ comments : "포함"
    articles ||--o{ favorites : "받음"
    articles ||--o{ article_tags : "가짐"
    
    tags ||--o{ article_tags : "연결됨"
```

### 데이터베이스 스키마

#### Users 테이블
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL,
    username VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    bio TEXT,
    image VARCHAR(500),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

#### Articles 테이블
```sql
CREATE TABLE articles (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    slug VARCHAR(255) UNIQUE NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    body TEXT NOT NULL,
    author_id INTEGER NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (author_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### Comments 테이블
```sql
CREATE TABLE comments (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    body TEXT NOT NULL,
    article_id INTEGER NOT NULL,
    author_id INTEGER NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (article_id) REFERENCES articles(id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES users(id) ON DELETE CASCADE
);
```

## 🔌 API 설계

### API 엔드포인트 구조

```mermaid
graph LR
    subgraph "인증 API"
        A1[POST /api/users/login]
        A2[POST /api/users]
        A3[GET /api/user]
        A4[PUT /api/user]
    end

    subgraph "게시글 API"
        B1[GET /api/articles]
        B2[POST /api/articles]
        B3[GET /api/articles/:slug]
        B4[PUT /api/articles/:slug]
        B5[DELETE /api/articles/:slug]
        B6[GET /api/articles/feed]
    end

    subgraph "댓글 API"
        C1[GET /api/articles/:slug/comments]
        C2[POST /api/articles/:slug/comments]
        C3[DELETE /api/articles/:slug/comments/:id]
    end

    subgraph "프로필 API"
        D1[GET /api/profiles/:username]
        D2[POST /api/profiles/:username/follow]
        D3[DELETE /api/profiles/:username/follow]
    end

    subgraph "기타 API"
        E1[GET /api/tags]
        E2[POST /api/articles/:slug/favorite]
        E3[DELETE /api/articles/:slug/favorite]
    end
```

### 인증 플로우

```mermaid
sequenceDiagram
    participant C as 클라이언트
    participant F as 프론트엔드
    participant A as API 서버
    participant D as 데이터베이스

    C->>F: 로그인 요청
    F->>A: POST /api/users/login
    A->>D: 사용자 인증 확인
    D-->>A: 사용자 정보
    A->>A: JWT 토큰 생성
    A-->>F: 토큰 + 사용자 정보
    F->>F: LocalStorage에 토큰 저장
    F-->>C: 로그인 완료

    Note over F: 이후 모든 요청에 토큰 포함

    C->>F: 보호된 리소스 요청
    F->>A: GET /api/user (Authorization: Bearer token)
    A->>A: JWT 토큰 검증
    A->>D: 사용자 정보 조회
    D-->>A: 사용자 정보
    A-->>F: 응답 데이터
    F-->>C: 화면 업데이트
```

## 🎨 UI/UX 설계

### 페이지 플로우

```mermaid
graph TD
    Start([시작]) --> Home[홈 페이지]
    
    Home --> Login{로그인 상태?}
    Login -->|No| LoginPage[로그인 페이지]
    Login -->|Yes| AuthHome[인증된 홈]
    
    LoginPage --> Register[회원가입 페이지]
    LoginPage -->|성공| AuthHome
    Register -->|성공| AuthHome
    
    AuthHome --> ArticleDetail[게시글 상세]
    AuthHome --> Editor[게시글 작성/수정]
    AuthHome --> Profile[프로필 페이지]
    AuthHome --> Settings[설정 페이지]
    
    ArticleDetail --> Comments[댓글 섹션]
    Editor -->|저장| ArticleDetail
    
    Profile --> UserArticles[사용자 게시글]
    Profile --> FavoriteArticles[좋아요 게시글]
    
    Settings --> Logout[로그아웃]
    Logout --> Home
```

### 컴포넌트 계층 구조

```mermaid
graph TD
    App[App] --> Router[Router]
    
    Router --> Layout[Layout]
    Layout --> Header[Header]
    Layout --> Main[Main Content]
    Layout --> Footer[Footer]
    
    Header --> Nav[Navigation]
    Header --> UserMenu[User Menu]
    
    Main --> HomePage[Home Page]
    Main --> ArticlePage[Article Page]
    Main --> EditorPage[Editor Page]
    Main --> ProfilePage[Profile Page]
    
    HomePage --> Banner[Banner]
    HomePage --> FeedTabs[Feed Tabs]
    HomePage --> ArticleList[Article List]
    HomePage --> TagList[Popular Tags]
    
    ArticleList --> ArticlePreview[Article Preview]
    
    ArticlePage --> ArticleMeta[Article Meta]
    ArticlePage --> ArticleContent[Article Content]
    ArticlePage --> CommentSection[Comment Section]
    
    CommentSection --> CommentList[Comment List]
    CommentSection --> CommentForm[Comment Form]
    CommentList --> CommentCard[Comment Card]
    
    EditorPage --> EditorForm[Editor Form]
    EditorForm --> TitleInput[Title Input]
    EditorForm --> DescriptionInput[Description Input]
    EditorForm --> BodyTextarea[Body Textarea]
    EditorForm --> TagInput[Tag Input]
```

## 🔐 보안 설계

### 인증 및 인가

```mermaid
graph TD
    subgraph "클라이언트 보안"
        A[XSS 방지]
        B[CSRF 보호]
        C[입력값 검증]
        D[토큰 안전 저장]
    end

    subgraph "서버 보안"
        E[JWT 검증]
        F[비밀번호 해싱]
        G[SQL Injection 방지]
        H[Rate Limiting]
    end

    subgraph "통신 보안"
        I[HTTPS 강제]
        J[CORS 설정]
        K[헤더 보안]
    end

    A --> E
    B --> F
    C --> G
    D --> H
    E --> I
    F --> J
    G --> K
```

### 데이터 보안 정책

1. **비밀번호 보안**
   - bcrypt를 사용한 해싱 (최소 12 rounds)
   - 평문 비밀번호는 메모리에서 즉시 삭제

2. **JWT 토큰 관리**
   - 액세스 토큰 만료 시간: 24시간
   - 서명 알고리즘: HS256
   - 토큰 재발급은 로그인을 통해서만

3. **API 보안**
   - 모든 민감한 엔드포인트는 인증 필요
   - 사용자는 본인 데이터만 수정 가능
   - 관리자 권한은 별도 구현하지 않음 (MVP 범위 외)

## 📁 프로젝트 구조

### 백엔드 구조 (Go)

```
backend/
├── cmd/
│   └── main.go                 # 애플리케이션 엔트리포인트
├── internal/
│   ├── config/                 # 설정 관리
│   ├── handlers/               # HTTP 핸들러
│   │   ├── user.go
│   │   ├── article.go
│   │   └── comment.go
│   ├── middleware/             # 미들웨어
│   │   ├── auth.go
│   │   ├── cors.go
│   │   └── logger.go
│   ├── models/                 # 데이터 모델
│   ├── repository/             # 데이터 액세스 계층
│   ├── services/               # 비즈니스 로직
│   └── utils/                  # 유틸리티
├── pkg/                        # 외부 공용 패키지
├── migrations/                 # DB 마이그레이션
├── go.mod
└── go.sum
```

### 프론트엔드 구조 (React)

```
frontend/
├── public/
├── src/
│   ├── components/             # 재사용 컴포넌트
│   │   ├── common/
│   │   ├── forms/
│   │   └── ui/
│   ├── pages/                  # 페이지 컴포넌트
│   │   ├── Home/
│   │   ├── Article/
│   │   ├── Editor/
│   │   ├── Profile/
│   │   └── Auth/
│   ├── hooks/                  # 커스텀 훅
│   ├── services/               # API 서비스
│   ├── contexts/               # Context API
│   ├── utils/                  # 유틸리티
│   ├── styles/                 # 스타일
│   ├── App.jsx
│   └── main.jsx
├── package.json
└── vite.config.js
```

## 🚀 배포 아키텍처

### 개발 환경

```mermaid
graph TB
    subgraph "개발 환경"
        Dev[개발자 머신]
        DevDB[(SQLite 파일)]
    end

    subgraph "Docker Compose"
        Frontend[Frontend Container<br/>React + Vite]
        Backend[Backend Container<br/>Go + Gin]
        Volume[Volume Mount<br/>Hot Reload]
    end

    Dev --> Volume
    Volume --> Frontend
    Volume --> Backend
    Backend --> DevDB
    Frontend -.->|API 호출| Backend
```

### 프로덕션 환경 (미래 확장)

```mermaid
graph TB
    subgraph "프로덕션 환경"
        LB[Load Balancer]
        Web[Web Server<br/>Nginx]
        App[App Server<br/>Go Binary]
        DB[(SQLite/PostgreSQL)]
        Static[Static Files]
    end

    Internet --> LB
    LB --> Web
    Web --> App
    Web --> Static
    App --> DB
```

## 📊 성능 최적화 전략

### 백엔드 최적화
- **데이터베이스 인덱스**: 자주 조회되는 필드에 인덱스 생성
- **쿼리 최적화**: N+1 문제 방지, 조인 최적화
- **응답 캐싱**: 정적 데이터 메모리 캐시
- **동시성**: Go의 goroutine 활용

### 프론트엔드 최적화
- **코드 스플리팅**: 라우트별 lazy loading
- **메모이제이션**: React.memo, useMemo 활용
- **번들 최적화**: Vite의 트리 쉐이킹
- **이미지 최적화**: 적절한 형식과 크기 사용

## 🔄 개발 워크플로우

### Git 브랜치 전략

```mermaid
gitgraph
    commit id: "Initial"
    branch develop
    commit id: "Setup"
    branch feature/auth
    commit id: "Auth API"
    commit id: "Auth UI"
    checkout develop
    merge feature/auth
    branch feature/articles
    commit id: "Article CRUD"
    commit id: "Article UI"
    checkout develop
    merge feature/articles
    checkout main
    merge develop id: "Release v1.0"
```

### 개발 단계별 마일스톤

1. **Phase 1: 환경 구성** (1주)
   - 프로젝트 구조 생성
   - Docker 환경 설정
   - 기본 라우팅 구현

2. **Phase 2: 핵심 기능** (2주)
   - 사용자 인증 구현
   - 게시글 CRUD 구현
   - 기본 UI 구현

3. **Phase 3: 추가 기능** (1주)
   - 댓글 시스템
   - 좋아요 기능
   - 프로필 시스템

## 📚 기술 스택 선택 이유

### 백엔드 - Go + Gin
- **단순성**: 명확한 문법, 적은 추상화
- **성능**: 컴파일 언어의 빠른 실행 속도
- **동시성**: 내장된 goroutine 지원
- **생태계**: 안정적이고 변화가 적은 라이브러리

### 프론트엔드 - React + Vite
- **학습 곡선**: 널리 사용되는 프레임워크
- **개발 경험**: Vite의 빠른 개발 서버
- **생태계**: 풍부한 라이브러리와 도구
- **취업 시장**: 높은 수요와 활용도

### 데이터베이스 - SQLite
- **단순성**: 파일 기반, 별도 서버 불필요
- **이식성**: 개발부터 배포까지 동일 환경
- **학습 용이성**: 복잡한 설정 없이 SQL 학습 가능
- **MVP 적합성**: 소규모 애플리케이션에 최적

---

이 설계 문서는 바이브 코딩 실습의 효율적인 학습을 위해 작성되었으며, 실제 구현 과정에서 필요에 따라 조정될 수 있습니다.