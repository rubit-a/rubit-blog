# Rubit's Blog 문서

Rubit's Blog 운영 및 배포를 위한 종합 가이드입니다.

## 📚 문서 목차

### 1. [개발 환경 설정](./01-getting-started.md)
- 프로젝트 설치 및 실행
- 사용 가능한 명령어
- 프로젝트 구조 이해
- 디버깅 방법

### 2. [블로그 포스트 작성](./02-writing-posts.md)
- 새 포스트 만들기
- Frontmatter 작성법
- Markdown 문법
- 이미지 추가
- 태그 사용
- MDX 활용

### 3. [Cloudflare Pages 배포](./03-deployment.md)
- GitHub 저장소 준비
- Cloudflare Pages 설정
- 커스텀 도메인 연결
- 자동 배포 설정
- 배포 문제 해결

### 4. [Giscus 댓글 설정](./04-giscus-setup.md)
- GitHub Discussions 활성화
- Giscus App 설치
- 블로그에 댓글 추가
- 테마 커스터마이징
- 댓글 관리

### 5. [블로그 커스터마이징](./05-customization.md)
- 사이트 기본 정보 변경
- 색상 테마 변경
- 폰트 변경
- 레이아웃 조정
- SEO 최적화
- 성능 최적화

### 6. [GitHub Actions 자동 배포](./06-github-actions.md)
- Cloudflare API 토큰 생성
- GitHub Secrets 설정
- 자동 배포 워크플로우
- 배포 테스트 및 문제 해결

## 🚀 빠른 시작

```bash
# 1. 의존성 설치
npm install

# 2. 개발 서버 실행
npm run dev

# 3. 브라우저에서 확인
# http://localhost:4321
```

## 📝 첫 포스트 작성하기

```bash
# 1. 새 포스트 파일 생성
touch src/content/blog/hello-world.md

# 2. Frontmatter와 내용 작성
# (02-writing-posts.md 참고)

# 3. 개발 서버에서 확인
npm run dev
```

## 🌐 배포하기

### 방법 1: GitHub Actions (권장)

```bash
# 1. GitHub Secrets 설정 (최초 1회)
# - CLOUDFLARE_API_TOKEN
# - CLOUDFLARE_ACCOUNT_ID
# - CLOUDFLARE_PROJECT_NAME

# 2. master 브랜치에 푸시
git add .
git commit -m "Add new post"
git push origin master

# 3. GitHub Actions가 자동으로 빌드 및 배포
# (06-github-actions.md 참고)
```

### 방법 2: Cloudflare Pages 직접 연동

```bash
# Cloudflare Dashboard에서 저장소 연결
# Push하면 자동 배포
# (03-deployment.md 참고)
```

## 💬 댓글 추가하기

1. GitHub Discussions 활성화
2. Giscus App 설치
3. Comments 컴포넌트 추가

자세한 내용은 [Giscus 설정 가이드](./04-giscus-setup.md)를 참고하세요.

## 🎨 커스터마이징

### 색상 변경

`src/styles/global.css`:

```css
:root {
  --accent: #YOUR_COLOR;
  --accent-dark: #YOUR_DARK_COLOR;
}
```

### 사이트 정보 변경

`src/consts.ts`:

```typescript
export const SITE_TITLE = "Your Blog Name";
export const SITE_DESCRIPTION = "Your description";
```

더 많은 커스터마이징 옵션은 [커스터마이징 가이드](./05-customization.md)를 참고하세요.

## 🛠 기술 스택

- **Framework**: [Astro](https://astro.build/)
- **Hosting**: [Cloudflare Pages](https://pages.cloudflare.com/)
- **Comments**: [Giscus](https://giscus.app/)
- **Styling**: CSS (Ruby Red Theme)
- **Content**: Markdown/MDX

## 📦 프로젝트 구조

```
rubit-blog/
├── docs/                   # 📚 문서 (현재 위치)
├── public/                 # 정적 파일
├── src/
│   ├── assets/            # 이미지 등 에셋
│   ├── components/        # 컴포넌트
│   │   ├── Sidebar.astro
│   │   ├── TagCloud.astro
│   │   └── RecentPosts.astro
│   ├── content/
│   │   └── blog/          # 📝 블로그 포스트
│   ├── layouts/
│   │   ├── BaseLayout.astro
│   │   └── BlogPost.astro
│   ├── pages/             # 페이지 라우트
│   └── styles/            # 전역 스타일
├── astro.config.mjs       # Astro 설정
└── package.json
```

## ✅ 체크리스트

### 초기 설정
- [ ] 개발 환경 설정
- [ ] 첫 포스트 작성
- [ ] 로컬에서 확인

### 배포
- [ ] GitHub 저장소 생성
- [ ] Cloudflare Pages 연결
- [ ] 배포 성공 확인
- [ ] 커스텀 도메인 설정 (선택)

### 기능 추가
- [ ] Giscus 댓글 설정
- [ ] About 페이지 작성
- [ ] SEO 메타 태그 확인
- [ ] Favicon 변경

### 커스터마이징
- [ ] 색상 테마 변경
- [ ] 폰트 선택
- [ ] 사이드바 커스터마이징
- [ ] Footer 정보 변경

## 🆘 문제 해결

### 개발 서버 오류
→ [개발 환경 설정 문서](./01-getting-started.md#디버깅) 참고

### 배포 실패
→ [배포 가이드 문제 해결](./03-deployment.md#배포-문제-해결) 참고

### 댓글이 표시되지 않음
→ [Giscus 문제 해결](./04-giscus-setup.md#문제-해결) 참고

## 📞 도움말

- [Astro 공식 문서](https://docs.astro.build/)
- [Cloudflare Pages 문서](https://developers.cloudflare.com/pages/)
- [Giscus 문서](https://giscus.app/ko)

## 📄 라이선스

이 프로젝트는 개인 블로그용으로 자유롭게 사용할 수 있습니다.
