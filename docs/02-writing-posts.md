# 블로그 포스트 작성 가이드

Rubit's Blog에서 새로운 포스트를 작성하는 방법을 안내합니다.

## 새 포스트 만들기

### 1. 파일 생성

`src/content/blog/` 디렉토리에 새 Markdown 파일을 생성합니다.

```bash
touch src/content/blog/my-first-post.md
```

**파일명 규칙:**
- 영문 소문자, 숫자, 하이픈(-) 사용
- 공백 대신 하이픈 사용
- 확장자: `.md` 또는 `.mdx`

### 2. Frontmatter 작성

포스트 상단에 메타데이터를 작성합니다.

```markdown
---
title: '나의 첫 블로그 포스트'
description: '이 포스트는 Rubit의 블로그에 대한 소개입니다.'
pubDate: '2024-01-07'
heroImage: '../../assets/my-image.jpg'
tags: ['astro', 'blogging', 'web']
---

포스트 내용을 여기에 작성합니다...
```

**필수 필드:**
- `title`: 포스트 제목
- `description`: 포스트 설명 (SEO 및 미리보기용)
- `pubDate`: 발행일 (YYYY-MM-DD 형식)

**선택 필드:**
- `updatedDate`: 수정일 (YYYY-MM-DD 형식)
- `heroImage`: 대표 이미지 경로
- `tags`: 태그 배열

## Markdown 작성

### 제목 (Headings)

```markdown
# H1 제목
## H2 제목
### H3 제목
```

**중요:** 목차(TOC)는 H2(`##`)와 H3(`###`)만 표시됩니다.

### 텍스트 서식

```markdown
**굵게**
*기울임*
~~취소선~~
`인라인 코드`
```

### 링크와 이미지

```markdown
[링크 텍스트](https://example.com)

![이미지 설명](../../assets/image.jpg)
```

### 코드 블록

````markdown
```javascript
function hello() {
  console.log('Hello, World!');
}
```
````

**지원 언어:** javascript, typescript, python, bash, css, html 등

### 인용구

```markdown
> 이것은 인용구입니다.
> 여러 줄로 작성할 수 있습니다.
```

### 목록

```markdown
- 순서 없는 목록
- 두 번째 항목
  - 중첩된 항목

1. 순서 있는 목록
2. 두 번째 항목
```

## 이미지 추가

### 1. 이미지 파일 추가

`src/assets/` 디렉토리에 이미지를 추가합니다.

```bash
cp my-image.jpg src/assets/
```

### 2. Frontmatter에서 사용

```markdown
---
heroImage: '../../assets/my-image.jpg'
---
```

### 3. 본문에서 사용

```markdown
![설명 텍스트](../../assets/my-image.jpg)
```

**이미지 최적화:**
- Astro가 자동으로 이미지를 최적화합니다
- WebP 형식으로 변환
- 반응형 이미지 생성

## 태그 사용

태그는 포스트를 분류하고 관련 콘텐츠를 그룹화합니다.

```markdown
---
tags: ['javascript', 'web-development', 'tutorial']
---
```

**태그 규칙:**
- 소문자 사용
- 여러 단어는 하이픈(-)으로 연결
- 의미 있는 태그 사용

## MDX 사용 (선택)

인터랙티브한 컴포넌트가 필요하면 `.mdx` 파일을 사용합니다.

```mdx
---
title: 'MDX 포스트'
description: '컴포넌트를 포함한 포스트'
pubDate: '2024-01-07'
---

import MyComponent from '../../components/MyComponent.astro';

# MDX 포스트

일반 Markdown 내용...

<MyComponent />

더 많은 내용...
```

## 포스트 미리보기

1. 개발 서버 실행
   ```bash
   npm run dev
   ```

2. 브라우저에서 확인
   - 목록: `http://localhost:4321/blog`
   - 개별 포스트: `http://localhost:4321/blog/파일명`

## 포스트 삭제

파일을 삭제하면 자동으로 블로그에서 제거됩니다.

```bash
rm src/content/blog/파일명.md
```

## 체크리스트

새 포스트를 작성할 때 확인하세요:

- [ ] Frontmatter의 모든 필수 필드 작성
- [ ] 제목이 명확하고 구체적인가?
- [ ] 설명(description)이 포스트를 잘 요약하는가?
- [ ] 태그가 적절한가?
- [ ] 이미지가 올바르게 표시되는가?
- [ ] 코드 블록에 언어가 지정되었는가?
- [ ] 맞춤법과 문법 확인

## 다음 단계

- [Cloudflare Pages 배포](./03-deployment.md)
- [Giscus 댓글 설정](./04-giscus-setup.md)
