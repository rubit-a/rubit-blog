# Cloudflare Pages 배포 가이드

Rubit's Blog를 Cloudflare Pages에 배포하는 방법을 안내합니다.

## 사전 준비

- GitHub 계정
- Cloudflare 계정 (무료)
- Git 저장소에 푸시된 코드

## 1. GitHub 저장소 준비

### 저장소 생성

1. GitHub에서 새 저장소 생성
2. 로컬 프로젝트를 Git 저장소로 초기화

```bash
cd rubit-blog
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

### 저장소 설정

- **Public 저장소 권장** (Giscus 댓글 사용 시 필수)
- `.gitignore` 확인 (`node_modules`, `dist`, `.astro` 제외)

## 2. Cloudflare Pages 설정

### 프로젝트 연결

1. [Cloudflare Dashboard](https://dash.cloudflare.com/) 로그인
2. **Workers & Pages** 섹션으로 이동
3. **Create application** 클릭
4. **Pages** 탭 선택
5. **Connect to Git** 클릭
6. GitHub 계정 연결 및 저장소 선택

### 빌드 설정

**Framework preset:** Astro

**Build settings:**
```
Build command: npm run build
Build output directory: dist
```

**Environment variables:** (선택)
```
NODE_VERSION: 18
```

### 배포 시작

1. **Save and Deploy** 클릭
2. 첫 배포가 자동으로 시작됩니다
3. 약 1-3분 소요

## 3. 배포 확인

### 배포 URL

배포가 완료되면 URL이 제공됩니다:
```
https://rubit-blog.pages.dev
```

### 배포 상태 확인

1. Cloudflare Dashboard > Workers & Pages
2. 프로젝트 선택
3. **Deployments** 탭에서 배포 로그 확인

## 4. 커스텀 도메인 연결 (선택)

### 도메인 추가

1. Cloudflare Pages 프로젝트 > **Custom domains**
2. **Set up a custom domain** 클릭
3. 도메인 입력 (예: `blog.example.com`)
4. DNS 레코드 추가 안내 따라하기

### DNS 설정

**Cloudflare DNS를 사용하는 경우:**
- 자동으로 CNAME 레코드 추가됨

**외부 DNS를 사용하는 경우:**
```
Type: CNAME
Name: blog
Target: rubit-blog.pages.dev
```

### SSL 인증서

- Cloudflare가 자동으로 무료 SSL 인증서 발급
- HTTPS 자동 활성화

## 5. 자동 배포 설정

### Git Push로 자동 배포

기본적으로 활성화되어 있습니다:

```bash
# 변경사항 커밋 및 푸시
git add .
git commit -m "Update blog post"
git push
```

- `main` 브랜치 푸시 → **Production** 배포
- 다른 브랜치 푸시 → **Preview** 배포

### Preview 배포

PR을 생성하면 자동으로 Preview URL이 생성됩니다:
```
https://abc123.rubit-blog.pages.dev
```

## 6. 빌드 설정 최적화

### package.json 확인

```json
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  }
}
```

### astro.config.mjs 확인

```javascript
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://your-domain.com', // 실제 도메인으로 변경
  // ...
});
```

## 7. 환경 변수 설정 (선택)

민감한 정보는 환경 변수로 관리:

1. Cloudflare Dashboard > 프로젝트 > **Settings**
2. **Environment variables** 섹션
3. 변수 추가

```
Name: API_KEY
Value: your-secret-key
```

코드에서 사용:
```javascript
const apiKey = import.meta.env.API_KEY;
```

## 8. 배포 문제 해결

### 빌드 실패

**오류 확인:**
1. Deployment 로그 확인
2. 로컬에서 `npm run build` 테스트

**일반적인 문제:**
- Node 버전 불일치 → 환경 변수에 `NODE_VERSION` 설정
- 의존성 오류 → `package-lock.json` 커밋 확인
- 타입 오류 → 로컬에서 먼저 수정

### 404 오류

**이미지 404:**
- `public/` 디렉토리의 파일은 `/파일명`으로 접근
- `src/assets/` 이미지는 Astro가 자동 처리

**페이지 404:**
- 라우팅 확인: `src/pages/` 구조
- 빌드 출력 확인: `dist/` 디렉토리

### 캐시 문제

브라우저 캐시 또는 Cloudflare 캐시:
```bash
# Cloudflare Dashboard에서 캐시 제거
# Pages 프로젝트 > Deployment > Retry deployment
```

## 9. 성능 최적화

### Cloudflare 설정

1. **Auto Minify** 활성화 (HTML, CSS, JS)
2. **Brotli** 압축 활성화
3. **Rocket Loader** (선택)

### Astro 최적화

이미지 최적화는 Astro가 자동 처리:
- WebP 변환
- 반응형 이미지
- 지연 로딩

## 10. 배포 체크리스트

- [ ] GitHub 저장소 생성 및 코드 푸시
- [ ] Cloudflare Pages 프로젝트 생성
- [ ] 빌드 설정 확인 (Astro preset)
- [ ] 첫 배포 성공 확인
- [ ] 배포 URL 접속 확인
- [ ] 커스텀 도메인 설정 (선택)
- [ ] SSL 인증서 확인
- [ ] 자동 배포 테스트 (git push)

## 다음 단계

- [Giscus 댓글 설정](./04-giscus-setup.md)
- [블로그 커스터마이징](./05-customization.md)

## 참고 자료

- [Cloudflare Pages 문서](https://developers.cloudflare.com/pages/)
- [Astro 배포 가이드](https://docs.astro.build/en/guides/deploy/cloudflare/)
