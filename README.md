# 🎯 Vibe Coding - RealWorld 실습 프로젝트

> **RealWorld**를 참고한 **바이브 코딩 실습 프로젝트**  
> Medium.com 클론을 통해 현실적인 풀스택 개발 경험을 쌓아보세요!

[![GitHub](https://img.shields.io/github/license/JoshuaSangwook/vibecoding)](https://github.com/JoshuaSangwook/vibecoding/blob/main/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/JoshuaSangwook/vibecoding)](https://github.com/JoshuaSangwook/vibecoding/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/JoshuaSangwook/vibecoding)](https://github.com/JoshuaSangwook/vibecoding/issues)

## 🌟 프로젝트 목적

이 프로젝트는 **RealWorld** 명세를 기반으로 한 **교육용 실습 프로젝트**입니다. 단순한 Todo 앱을 넘어서 실제 운영되는 웹 애플리케이션과 유사한 복잡도를 가진 프로젝트를 통해:

- 📚 **실무 중심 학습**: 현실적인 개발 시나리오 경험
- 🛠️ **모던 기술 스택**: Go + React 조합으로 현대적 개발 방식 학습  
- 🎨 **바이브 코딩**: 효율적이고 즐거운 코딩 경험 추구
- 🔄 **점진적 구현**: MVP부터 고급 기능까지 단계별 발전

## ✨ 핵심 기능

### 📝 **게시글 시스템**
- 게시글 작성, 수정, 삭제
- Markdown 렌더링 지원
- 태그 시스템

### 👥 **사용자 시스템**
- JWT 기반 인증
- 프로필 관리
- 팔로우/언팔로우

### 💬 **소셜 기능**
- 댓글 시스템
- 좋아요 기능
- 개인 피드

### 📱 **사용자 경험**
- 반응형 디자인
- SPA (Single Page Application)
- 직관적인 UI/UX

## 🔧 기술 스택

### 🖥️ **백엔드**
- **언어**: [Go](https://golang.org/)
- **프레임워크**: [Gin](https://gin-gonic.com/)
- **데이터베이스**: SQLite
- **인증**: JWT

### 🎨 **프론트엔드**
- **프레임워크**: [React 18](https://reactjs.org/)
- **빌드 도구**: [Vite](https://vitejs.dev/)
- **스타일링**: [Tailwind CSS](https://tailwindcss.com/)
- **라우팅**: [React Router](https://reactrouter.com/)

### 🐳 **개발/배포**
- **컨테이너**: Docker & docker-compose
- **버전 관리**: Git & GitHub

## 🚀 빠른 시작

### 📋 사전 요구사항
- [Docker](https://www.docker.com/) & Docker Compose
- [Git](https://git-scm.com/)
- [Go 1.21+](https://golang.org/) (로컬 개발시)
- [Node.js 18+](https://nodejs.org/) (로컬 개발시)

### 📥 설치 및 실행

1. **저장소 클론**
   ```bash
   git clone https://github.com/JoshuaSangwook/vibecoding.git
   cd vibecoding
   ```

2. **Docker로 실행** (권장)
   ```bash
   docker-compose up -d
   ```

3. **애플리케이션 확인**
   - 프론트엔드: http://localhost:3000
   - 백엔드 API: http://localhost:8080/api

### 🛠️ 로컬 개발

1. **백엔드 실행**
   ```bash
   cd backend
   go mod download
   go run cmd/main.go
   ```

2. **프론트엔드 실행**
   ```bash
   cd frontend
   npm install
   npm run dev
   ```

## 📚 프로젝트 구조

```
vibecoding/
├── backend/           # Go 백엔드
│   ├── cmd/          # 애플리케이션 엔트리포인트
│   ├── internal/     # 내부 패키지
│   └── pkg/          # 공용 패키지
├── frontend/         # React 프론트엔드
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   └── hooks/
│   └── public/
├── docs/             # 프로젝트 문서
├── docker/           # Docker 설정
└── README.md
```

## 📖 문서

- 📋 [구현 작업 목록](./docs/tasks.md) - MVP 구현을 위한 단계별 작업 목록
- 📄 [PRD 문서](./docs/PRD.md) - 제품 요구사항 정의서
- 🔧 [개발 가이드](./docs/development.md) - 개발 환경 구성 및 가이드 (예정)

## 🎯 구현 단계

### Phase 1: 기본 환경 구성 ✅
- [x] 프로젝트 구조 설정
- [x] 기본 문서 작성
- [ ] Docker 환경 구성

### Phase 2: 핵심 기능 구현 🚧
- [ ] 사용자 인증 시스템
- [ ] 게시글 CRUD 시스템
- [ ] 댓글 시스템

### Phase 3: 추가 기능 📝
- [ ] 사용자 프로필
- [ ] 태그 시스템
- [ ] 좋아요 기능

### Phase 4: 배포 준비 🚀
- [ ] Docker 설정 완성
- [ ] 에러 처리 개선

## 🤝 기여하기

이 프로젝트는 교육용 실습 프로젝트입니다. 기여를 환영합니다!

1. 이 저장소를 Fork하세요
2. 새로운 기능 브랜치를 생성하세요 (`git checkout -b feature/amazing-feature`)
3. 변경사항을 커밋하세요 (`git commit -m '새로운 기능 추가'`)
4. 브랜치에 푸시하세요 (`git push origin feature/amazing-feature`)
5. Pull Request를 열어주세요

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

## 🙏 감사의 말

- [RealWorld](https://github.com/gothinkster/realworld) - 프로젝트 영감과 API 명세 제공
- [Conduit](https://demo.realworld.io/) - 참고 구현체

## 🔗 관련 링크

- [RealWorld 공식 문서](https://realworld-docs.netlify.app/)
- [RealWorld API 명세](https://realworld-docs.netlify.app/docs/specs/backend-specs/introduction)
- [이 프로젝트의 GitHub](https://github.com/JoshuaSangwook/vibecoding)

---

**바이브 코딩으로 즐겁게 배워보세요! 🎉**