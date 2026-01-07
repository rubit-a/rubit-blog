# GitHub Actions를 통한 자동 배포 설정

GitHub Actions를 사용하여 master 브랜치에 push할 때 자동으로 Cloudflare Pages에 배포하는 방법을 안내합니다.

## 📋 사전 준비

- GitHub 저장소
- Cloudflare 계정
- Cloudflare Pages 프로젝트

## 1. Cloudflare API 토큰 생성

### 1-1. Cloudflare Dashboard 접속

1. [Cloudflare Dashboard](https://dash.cloudflare.com/) 로그인
2. 우측 상단 프로필 클릭
3. **My Profile** 선택
4. 왼쪽 메뉴에서 **API Tokens** 클릭

### 1-2. API 토큰 생성

1. **Create Token** 클릭
2. **Edit Cloudflare Workers** 템플릿 선택 (또는 Custom token)
3. 권한 설정:
   - **Account** > **Cloudflare Pages** > **Edit**
4. **Continue to summary** 클릭
5. **Create Token** 클릭
6. 생성된 토큰 복사 (한 번만 표시됨!)

**중요:** 토큰을 안전하게 보관하세요. 다시 볼 수 없습니다.

## 2. Cloudflare Account ID 확인

### 방법 1: Dashboard에서 확인

1. [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. **Workers & Pages** 클릭
3. 우측 사이드바에서 **Account ID** 확인 및 복사

### 방법 2: Pages 프로젝트에서 확인

1. Pages 프로젝트 선택
2. URL에서 Account ID 확인:
   ```
   https://dash.cloudflare.com/{ACCOUNT_ID}/pages/...
   ```

## 3. Cloudflare 프로젝트 이름 확인

1. Cloudflare Dashboard > **Workers & Pages**
2. 프로젝트 목록에서 프로젝트 이름 확인

예: `rubit-blog`

**또는** 새 프로젝트를 생성할 이름을 정합니다.

## 4. GitHub Secrets 설정

### 4-1. GitHub 저장소 이동

GitHub 저장소 > **Settings** > **Secrets and variables** > **Actions**

### 4-2. Secrets 추가

**New repository secret** 버튼을 클릭하여 다음 3개의 secret을 추가합니다:

#### Secret 1: CLOUDFLARE_API_TOKEN

```
Name: CLOUDFLARE_API_TOKEN
Secret: (1단계에서 생성한 API 토큰)
```

#### Secret 2: CLOUDFLARE_ACCOUNT_ID

```
Name: CLOUDFLARE_ACCOUNT_ID
Secret: (2단계에서 확인한 Account ID)
```

#### Secret 3: CLOUDFLARE_PROJECT_NAME

```
Name: CLOUDFLARE_PROJECT_NAME
Secret: rubit-blog (또는 실제 프로젝트 이름)
```

### 4-3. Secrets 확인

3개의 secret이 모두 추가되었는지 확인:

- ✅ CLOUDFLARE_API_TOKEN
- ✅ CLOUDFLARE_ACCOUNT_ID
- ✅ CLOUDFLARE_PROJECT_NAME

## 5. GitHub Actions 워크플로우 확인

프로젝트에 이미 `.github/workflows/deploy.yml` 파일이 생성되어 있습니다.

### 워크플로우 파일 내용

```yaml
name: Deploy to Cloudflare Pages

on:
  push:
    branches:
      - master
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      deployments: write

    name: Deploy to Cloudflare Pages
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Publish to Cloudflare Pages
        uses: cloudflare/pages-action@v1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: ${{ secrets.CLOUDFLARE_PROJECT_NAME }}
          directory: dist
          gitHubToken: ${{ secrets.GITHUB_TOKEN }}
          branch: ${{ github.ref_name }}
          wranglerVersion: '3'
```

### 워크플로우 동작

1. `master` 또는 `main` 브랜치에 push 시 자동 실행
2. Node.js 18 환경 설정
3. 의존성 설치
4. Astro 빌드 실행
5. Cloudflare Pages에 배포
   - Wrangler 3 사용 (프로젝트가 없으면 자동 생성)

## 6. 배포 테스트

### 6-1. 코드 변경 및 커밋

```bash
# 코드 변경
echo "# Test" >> README.md

# 커밋
git add .
git commit -m "Test GitHub Actions deployment"

# Push to master
git push origin master
```

### 6-2. GitHub Actions 확인

1. GitHub 저장소 > **Actions** 탭
2. 실행 중인 워크플로우 확인
3. 클릭하여 상세 로그 확인

### 6-3. 배포 결과 확인

- ✅ 모든 단계가 성공 (녹색 체크)
- 🌐 Cloudflare Pages에서 배포 확인
- 🔗 배포 URL 접속 확인

## 7. 배포 URL 확인

### GitHub Actions 로그에서

워크플로우 실행 로그의 "Publish to Cloudflare Pages" 단계에서 배포 URL을 확인할 수 있습니다.

### Cloudflare Dashboard에서

1. Cloudflare Dashboard > **Workers & Pages**
2. 프로젝트 선택
3. **Deployments** 탭에서 최신 배포 확인
4. 배포 URL 클릭

## 8. 문제 해결

### 워크플로우 실패 시

#### "Unauthorized" 오류

**원인:** API 토큰이 잘못되었거나 권한이 부족

**해결:**
1. Cloudflare API 토큰 재생성
2. **Cloudflare Pages** 권한 확인
3. GitHub Secret 업데이트

#### "Project not found" 오류

**원인:** 프로젝트가 아직 생성되지 않았거나 이름이 잘못됨

**해결:**
1. `wranglerVersion: '3'`이 설정되어 있는지 확인 (자동으로 프로젝트 생성)
2. 또는 Cloudflare Dashboard에서 수동으로 프로젝트 생성
3. 프로젝트 이름이 `CLOUDFLARE_PROJECT_NAME` Secret과 일치하는지 확인

#### "Build failed" 오류

**원인:** 빌드 과정에서 오류 발생

**해결:**
1. 로컬에서 `npm run build` 테스트
2. 오류 수정 후 다시 push

#### Node 버전 오류

**원인:** Node.js 버전 불일치

**해결:**
`.github/workflows/deploy.yml`에서 Node 버전 변경:

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'  # 또는 다른 버전
```

### 로그 확인 방법

1. GitHub > Actions 탭
2. 실패한 워크플로우 클릭
3. 각 단계 클릭하여 상세 로그 확인
4. 빨간색 오류 메시지 확인

## 9. 고급 설정

### 브랜치별 배포 환경 분리

```yaml
on:
  push:
    branches:
      - main        # Production
      - develop     # Staging
```

### 배포 알림 추가

Slack, Discord 등으로 배포 완료 알림을 받을 수 있습니다.

```yaml
- name: Notify Slack
  if: success()
  uses: slackapi/slack-github-action@v1
  with:
    webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### 캐시 최적화

의존성 설치 시간 단축:

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '18'
    cache: 'npm'  # 이미 설정됨
```

## 10. 보안 권장사항

### API 토큰 관리

- ✅ GitHub Secrets에만 저장
- ✅ 최소 권한 원칙 적용
- ✅ 정기적으로 토큰 갱신
- ❌ 코드에 직접 작성 금지
- ❌ 공개 저장소에 노출 금지

### 브랜치 보호

1. GitHub 저장소 > **Settings** > **Branches**
2. **Add rule** 클릭
3. Branch name pattern: `master`
4. 옵션 설정:
   - ✅ Require status checks to pass
   - ✅ Require branches to be up to date

## 11. 배포 워크플로우

### 일반적인 흐름

```
1. 로컬에서 코드 작성/수정
   ↓
2. git add & commit
   ↓
3. git push origin master
   ↓
4. GitHub Actions 자동 실행
   - 코드 체크아웃
   - 의존성 설치
   - 빌드
   - Cloudflare Pages 배포
   ↓
5. 배포 완료 (약 2-3분)
   ↓
6. 웹사이트 자동 업데이트
```

## 12. 배포 체크리스트

배포 전 확인사항:

- [ ] Cloudflare API 토큰 생성
- [ ] Cloudflare Account ID 확인
- [ ] Cloudflare 프로젝트 이름 확인
- [ ] GitHub Secrets 3개 모두 추가
- [ ] `.github/workflows/deploy.yml` 파일 존재
- [ ] 로컬에서 빌드 테스트 성공
- [ ] master 브랜치에 push
- [ ] GitHub Actions 워크플로우 성공 확인
- [ ] Cloudflare Pages에서 배포 확인
- [ ] 웹사이트 정상 작동 확인

## 참고 자료

- [GitHub Actions 문서](https://docs.github.com/en/actions)
- [Cloudflare Pages Actions](https://github.com/cloudflare/pages-action)
- [Cloudflare API 토큰 문서](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)

---

이제 master 브랜치에 push할 때마다 자동으로 Cloudflare Pages에 배포됩니다!
