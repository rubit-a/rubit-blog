# 개발 환경 설정

Rubit's Blog 개발 환경을 설정하는 방법을 안내합니다.

## 요구 사항

- Node.js 18.0 이상
- npm 또는 yarn
- Git

## 설치

### 1. 저장소 클론

```bash
git clone <repository-url>
cd rubit-blog
```

### 2. 의존성 설치

```bash
npm install
```

### 3. 개발 서버 실행

```bash
npm run dev
```

개발 서버가 `http://localhost:4321`에서 실행됩니다.

## 사용 가능한 명령어

| 명령어 | 설명 |
|--------|------|
| `npm run dev` | 개발 서버 시작 (Hot reload) |
| `npm run build` | 프로덕션 빌드 생성 (`dist/` 폴더) |
| `npm run preview` | 빌드된 사이트 미리보기 |
| `npm run astro` | Astro CLI 명령어 실행 |

## 프로젝트 구조

```
rubit-blog/
├── public/              # 정적 파일 (이미지, 폰트 등)
├── src/
│   ├── assets/         # Astro가 처리하는 에셋 (최적화됨)
│   ├── components/     # 재사용 가능한 컴포넌트
│   ├── content/        # 블로그 포스트 (Markdown/MDX)
│   ├── layouts/        # 페이지 레이아웃
│   ├── pages/          # 라우트 페이지
│   ├── styles/         # 전역 스타일
│   └── consts.ts       # 사이트 설정
├── docs/               # 문서
└── astro.config.mjs    # Astro 설정
```

## 디버깅

### 빌드 오류

빌드 중 오류가 발생하면:

1. `node_modules` 삭제 후 재설치
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

2. 캐시 클리어
   ```bash
   rm -rf .astro dist
   npm run build
   ```

### 개발 서버 오류

- 포트 충돌: `package.json`에서 포트 변경
- Hot reload 작동 안 함: 브라우저 새로고침 (Cmd/Ctrl + R)

## 다음 단계

- [블로그 포스트 작성하기](./02-writing-posts.md)
- [Cloudflare Pages 배포](./03-deployment.md)
