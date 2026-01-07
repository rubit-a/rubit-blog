# Giscus 댓글 설정 가이드

Giscus를 사용하여 GitHub Discussions 기반 댓글 시스템을 추가하는 방법을 안내합니다.

## Giscus란?

- GitHub Discussions를 활용한 댓글 시스템
- GitHub 계정으로 로그인하여 댓글 작성
- 완전 무료, 광고 없음
- Markdown 지원

## 1. GitHub 저장소 설정

### 1-1. 저장소 Public으로 설정

Giscus는 Public 저장소에서만 작동합니다.

1. GitHub 저장소 > **Settings**
2. **Danger Zone** > **Change repository visibility**
3. **Make public** 선택

### 1-2. Discussions 활성화

1. GitHub 저장소 > **Settings**
2. **Features** 섹션
3. **Discussions** 체크박스 활성화

### 1-3. Discussion 카테고리 생성

1. 저장소 > **Discussions** 탭
2. 카테고리 관리 (오른쪽 설정 아이콘)
3. **New category** 클릭
4. 카테고리 생성:
   - **Name:** `Comments` 또는 `댓글`
   - **Description:** 블로그 댓글
   - **Format:** `Announcement` (권장)

## 2. Giscus App 설치

1. [Giscus App](https://github.com/apps/giscus) 페이지 이동
2. **Install** 클릭
3. 설치할 저장소 선택
   - **Only select repositories** 선택
   - 블로그 저장소 선택
4. **Install** 확인

## 3. Giscus 설정 생성

### 3-1. Giscus 설정 페이지

1. [giscus.app](https://giscus.app/ko) 방문
2. 한국어 선택 (우측 상단)

### 3-2. 저장소 정보 입력

**저장소:** `사용자명/저장소명`

예: `ruby/rubit-blog`

입력 후 저장소 조건 확인:
- ✅ 저장소가 public입니다
- ✅ giscus 앱이 설치되어 있습니다
- ✅ Discussions 기능이 활성화되어 있습니다

### 3-3. 매핑 방식 선택

**권장:** `pathname`

- 페이지 경로를 기반으로 Discussion 생성
- URL 변경 시 댓글 유지
- 가장 안정적인 방식

### 3-4. Discussion 카테고리 선택

생성한 카테고리 선택 (예: `Comments`)

### 3-5. 기타 설정

- **Discussion 제목 형식:** `pathname`
- **테마:** `preferred_color_scheme` (자동)
- **lazy loading 활성화:** 체크 (성능 향상)

### 3-6. 스크립트 코드 복사

페이지 하단에 생성된 스크립트를 복사합니다:

```html
<script src="https://giscus.app/client.js"
        data-repo="ruby/rubit-blog"
        data-repo-id="R_kgDO..."
        data-category="Comments"
        data-category-id="DIC_kwDO..."
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="ko"
        crossorigin="anonymous"
        async>
</script>
```

## 4. 블로그에 Giscus 추가

### 4-1. Comments 컴포넌트 생성

새 파일 생성: `src/components/Comments.astro`

```astro
---
// src/components/Comments.astro
---

<div class="comments">
  <script src="https://giscus.app/client.js"
          data-repo="YOUR_USERNAME/YOUR_REPO"
          data-repo-id="YOUR_REPO_ID"
          data-category="Comments"
          data-category-id="YOUR_CATEGORY_ID"
          data-mapping="pathname"
          data-strict="0"
          data-reactions-enabled="1"
          data-emit-metadata="0"
          data-input-position="bottom"
          data-theme="preferred_color_scheme"
          data-lang="ko"
          crossorigin="anonymous"
          async>
  </script>
</div>

<style>
  .comments {
    margin-top: 4rem;
    padding-top: 2rem;
    border-top: 1px solid rgb(var(--gray-light));
  }
</style>
```

**중요:** `YOUR_USERNAME`, `YOUR_REPO_ID` 등을 실제 값으로 변경하세요.

### 4-2. BlogPost 레이아웃에 추가

`src/layouts/BlogPost.astro` 파일 수정:

```astro
---
import Comments from '../components/Comments.astro';
// ... 기존 imports
---

<html lang="ko">
  <!-- ... 기존 코드 -->
  <body>
    <!-- ... 기존 코드 -->
    <main>
      <article>
        <!-- 포스트 내용 -->
        <slot />
      </article>

      <!-- 댓글 추가 -->
      <Comments />
    </main>
    <!-- ... -->
  </body>
</html>
```

## 5. 테마 커스터마이징

### 루비 테마에 맞추기

`Comments.astro`에서 테마 변경:

```html
data-theme="light"
```

또는 다크모드 지원:

```html
data-theme="preferred_color_scheme"
```

**사용 가능한 테마:**
- `light` - 밝은 테마
- `dark` - 어두운 테마
- `preferred_color_scheme` - 시스템 설정
- `dark_dimmed` - GitHub 스타일
- 기타 커스텀 CSS

## 6. 댓글 관리

### Discussion에서 댓글 관리

1. GitHub 저장소 > **Discussions** 탭
2. 각 포스트의 Discussion 확인
3. 부적절한 댓글 삭제/숨김 가능

### 댓글 권한 설정

**Discussion 카테고리 설정:**
- Announcement 형식: 포스트 작성자만 Discussion 생성 가능
- 일반 형식: 누구나 Discussion 생성 가능

## 7. 문제 해결

### 댓글이 표시되지 않음

**확인 사항:**
1. 저장소가 Public인가?
2. Discussions가 활성화되었는가?
3. Giscus App이 설치되었는가?
4. `data-repo-id`와 `data-category-id`가 정확한가?

**해결 방법:**
- 브라우저 콘솔에서 오류 확인
- giscus.app에서 설정 다시 생성
- 저장소 이름 확인 (대소문자 구분)

### 댓글이 다른 페이지에 표시됨

**원인:** 매핑 방식 문제

**해결:**
- `data-mapping="pathname"` 사용 권장
- URL 경로가 고유한지 확인

### 스타일 깨짐

Giscus iframe CSS 커스터마이징:

```css
.giscus, .giscus-frame {
  width: 100%;
}

.giscus-frame {
  border: none;
}
```

## 8. 설정 체크리스트

- [ ] GitHub 저장소 Public으로 설정
- [ ] Discussions 활성화
- [ ] Comments 카테고리 생성
- [ ] Giscus App 설치
- [ ] giscus.app에서 설정 생성
- [ ] Comments.astro 컴포넌트 생성
- [ ] BlogPost 레이아웃에 추가
- [ ] 테스트 댓글 작성 확인

## 9. 댓글 정책 (권장)

블로그에 댓글 정책을 명시하세요:

1. 존중과 예의
2. 스팸 금지
3. 개인정보 보호
4. 저작권 존중

## 다음 단계

- [블로그 커스터마이징](./05-customization.md)

## 참고 자료

- [Giscus 공식 문서](https://giscus.app/ko)
- [GitHub Discussions](https://docs.github.com/en/discussions)
