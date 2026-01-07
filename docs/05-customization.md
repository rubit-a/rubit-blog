# 블로그 커스터마이징 가이드

Rubit's Blog를 개인 취향에 맞게 커스터마이징하는 방법을 안내합니다.

## 1. 사이트 기본 정보 변경

### 사이트 제목과 설명

`src/consts.ts` 파일 수정:

```typescript
export const SITE_TITLE = "Rubit's Blog";
export const SITE_DESCRIPTION = '개발과 기술에 대한 생각을 기록하는 공간입니다.';
```

### 사이트 URL 설정

`astro.config.mjs` 파일 수정:

```javascript
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://your-domain.com',
  // ...
});
```

**중요:** 실제 배포 URL로 변경하세요 (RSS 피드, Sitemap에 사용됨)

## 2. 색상 테마 변경

### 메인 색상 변경

`src/styles/global.css` 파일 수정:

```css
:root {
  /* 현재 Ruby Red Theme */
  --accent: #C41E3A;
  --accent-dark: #8B0000;
  --gray-light: 240, 230, 232;
}
```

### 다른 색상 예시

**Blue Theme:**
```css
--accent: #2563eb;
--accent-dark: #1e40af;
--gray-light: 229, 233, 240;
```

**Green Theme:**
```css
--accent: #059669;
--accent-dark: #047857;
--gray-light: 230, 240, 235;
```

**Purple Theme:**
```css
--accent: #7c3aed;
--accent-dark: #5b21b6;
--gray-light: 235, 230, 240;
```

## 3. 폰트 변경

### 시스템 폰트 사용

`src/styles/global.css`:

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI',
               'Noto Sans KR', sans-serif;
}
```

### 웹 폰트 추가 (Google Fonts)

1. `src/components/BaseHead.astro` 수정:

```astro
<head>
  <!-- ... 기존 코드 -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet">
</head>
```

2. `src/styles/global.css` 수정:

```css
body {
  font-family: 'Noto Sans KR', sans-serif;
}
```

## 4. 사이드바 커스터마이징

### 프로필 이미지 추가

`src/components/Sidebar.astro`:

```astro
<aside class="sidebar">
  <div class="sidebar-header">
    <img src="/profile.jpg" alt="프로필" class="profile-image" />
    <h2><a href="/">{SITE_TITLE}</a></h2>
  </div>
  <!-- ... -->
</aside>

<style>
  .profile-image {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    margin-bottom: 1rem;
  }
</style>
```

### 소셜 링크 추가

```astro
<div class="social-links">
  <a href="https://github.com/your-username" target="_blank">GitHub</a>
  <a href="https://twitter.com/your-username" target="_blank">Twitter</a>
</div>
```

### 사이드바 순서 변경

`src/components/Sidebar.astro`에서 컴포넌트 순서 조정:

```astro
<aside class="sidebar">
  <div class="sidebar-header">...</div>

  <!-- 순서 변경 가능 -->
  <TagCloud />
  <RecentPosts />
  <nav class="sidebar-nav">...</nav>
</aside>
```

## 5. Footer 커스터마이징

### 저작권 정보 변경

`src/components/Footer.astro`:

```astro
<footer>
  &copy; {today.getFullYear()} Your Name. All rights reserved.
  <!-- 또는 -->
  &copy; {today.getFullYear()} Rubit's Blog. Made with Astro.
</footer>
```

### 소셜 링크 변경

```astro
<div class="social-links">
  <a href="https://github.com/your-username" target="_blank">
    <!-- GitHub 아이콘 -->
  </a>
  <!-- 원하는 소셜 추가/제거 -->
</div>
```

## 6. 레이아웃 조정

### 사이드바 너비 변경

`src/components/Sidebar.astro`:

```css
.sidebar {
  width: 220px;  /* 원하는 너비로 변경 */
}
```

### 콘텐츠 영역 너비 변경

`src/layouts/BaseLayout.astro`:

```css
main {
  max-width: 1200px;  /* 원하는 너비로 변경 */
}
```

### 모바일 브레이크포인트 변경

```css
@media (max-width: 960px) {  /* 원하는 값으로 변경 */
  .sidebar {
    display: none;
  }
}
```

## 7. About 페이지 커스터마이징

`src/pages/about.astro` 수정:

```astro
<BaseLayout title="About Me | Rubit's Blog" description="Rubit Blog 소개">
  <section class="about">
    <h1>About Me</h1>

    <!-- 자신의 소개 작성 -->
    <p>안녕하세요, 루빗입니다...</p>

    <!-- 경력, 기술 스택, 취미 등 -->
    <h2>Skills</h2>
    <ul>
      <li>JavaScript/TypeScript</li>
      <li>React, Astro</li>
      <!-- ... -->
    </ul>
  </section>
</BaseLayout>
```

## 8. 홈페이지 커스터마이징

`src/pages/index.astro` 전체 수정:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import { SITE_DESCRIPTION, SITE_TITLE } from '../consts';
---

<BaseLayout title={SITE_TITLE} description={SITE_DESCRIPTION}>
  <section class="hero">
    <h1>Welcome to Rubit's Blog</h1>
    <p>개발과 기술에 대한 이야기를 나눕니다.</p>
  </section>

  <!-- 최근 포스트, 인기 포스트 등 추가 -->
</BaseLayout>
```

## 9. 코드 블록 스타일링

### Syntax Highlighting 테마 변경

Astro는 기본적으로 Shiki를 사용합니다.

`astro.config.mjs`:

```javascript
export default defineConfig({
  markdown: {
    shikiConfig: {
      theme: 'nord',  // 원하는 테마로 변경
      // 'dracula', 'github-dark', 'monokai', 등
    },
  },
});
```

### 코드 블록 커스텀 스타일

`src/styles/global.css`:

```css
pre {
  padding: 1.5em;
  border-radius: 8px;
  overflow-x: auto;
}

code {
  font-family: 'Fira Code', 'Consolas', monospace;
  font-size: 0.9em;
}
```

## 10. SEO 최적화

### Open Graph 이미지

`src/components/BaseHead.astro`:

```astro
<meta property="og:image" content="/og-image.jpg" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

`public/og-image.jpg` 파일 추가 (1200x630px 권장)

### Favicon 변경

1. Favicon 생성 (favicon.ico, icon-192.png, icon-512.png)
2. `public/` 디렉토리에 추가
3. `src/components/BaseHead.astro` 확인

## 11. 추가 기능

### 검색 기능 추가

Algolia DocSearch 또는 Pagefind 사용

### RSS 피드 커스터마이징

`src/pages/rss.xml.js` 수정

### Sitemap 설정

`astro.config.mjs`에 sitemap 통합 추가됨

### Analytics 추가

Google Analytics, Cloudflare Web Analytics 등

## 12. 성능 최적화

### 이미지 최적화

Astro Image 사용 (이미 적용됨):

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/image.jpg';
---

<Image src={myImage} alt="설명" width={800} height={600} />
```

### 프리로드 추가

중요한 리소스 프리로드:

```astro
<link rel="preload" href="/fonts/main.woff2" as="font" type="font/woff2" crossorigin>
```

## 13. 커스터마이징 체크리스트

- [ ] 사이트 제목과 설명 변경
- [ ] 색상 테마 설정
- [ ] 폰트 선택
- [ ] Footer 저작권 정보
- [ ] About 페이지 작성
- [ ] 홈페이지 커스터마이징
- [ ] Favicon 변경
- [ ] OG 이미지 추가
- [ ] 소셜 링크 추가

## 참고 자료

- [Astro 문서](https://docs.astro.build/)
- [Astro Themes](https://astro.build/themes/)
- [Google Fonts](https://fonts.google.com/)
