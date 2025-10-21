# 환경 변수 설정 가이드

이 문서는 로컬 개발 환경과 프로덕션(Vercel) 환경에서 환경 변수를 올바르게 설정하는 방법을 설명합니다.

## 목차
1. [필수 환경 변수](#필수-환경-변수)
2. [로컬 개발 환경 설정](#로컬-개발-환경-설정)
3. [Vercel 프로덕션 환경 설정](#vercel-프로덕션-환경-설정)
4. [Supabase Dashboard 설정](#supabase-dashboard-설정)
5. [문제 해결](#문제-해결)

---

## 필수 환경 변수

### 1. `NEXT_PUBLIC_SITE_URL`
**용도:** OAuth 및 이메일 인증 콜백 URL의 기본 도메인
- **로컬:** `http://localhost:3000`
- **프로덕션:** `https://your-domain.com`

⚠️ **중요:** 마지막 슬래시(`/`) 없이 입력

### 2. `NEXT_PUBLIC_SUPABASE_URL`
**용도:** Supabase 프로젝트 URL
- Supabase Dashboard → Settings → API에서 확인
- 예: `https://your-project-id.supabase.co`

### 3. `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
**용도:** Supabase 공개 키 (anon public key)
- Supabase Dashboard → Settings → API에서 확인

### 4. `SUPABASE_SERVICE_ROLE_KEY` (선택)
**용도:** 관리자 권한 작업 (RLS 우회)
- ⚠️ **절대 클라이언트 코드에 노출 금지**
- ⚠️ **Git에 커밋 금지**

---

## 로컬 개발 환경 설정

### 1. `.env.local` 파일 생성

프로젝트 루트에 `.env.local` 파일을 생성하고 다음 내용을 입력합니다:

```bash
# Supabase 설정
NEXT_PUBLIC_SUPABASE_URL=https://your-project-id.supabase.co
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=your-anon-public-key-here

# Site URL (로컬)
NEXT_PUBLIC_SITE_URL=http://localhost:3000

# (선택) Service Role Key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key-here

# 환경 구분
NODE_ENV=development
```

### 2. `.env.local` Git에서 제외 확인

`.gitignore`에 다음 내용이 포함되어 있는지 확인:

```
.env.local
.env*.local
```

### 3. 로컬 서버 재시작

환경 변수 변경 후 개발 서버를 재시작합니다:

```bash
yarn dev
```

---

## Vercel 프로덕션 환경 설정

### 1. Vercel Dashboard 접속

1. https://vercel.com/dashboard 접속
2. 배포된 프로젝트 선택

### 2. Environment Variables 추가

**Settings → Environment Variables**로 이동 후 다음 변수들을 추가:

| Name | Value | Environment |
|------|-------|-------------|
| `NEXT_PUBLIC_SITE_URL` | `https://your-domain.vercel.app` | Production, Preview, Development |
| `NEXT_PUBLIC_SUPABASE_URL` | `https://your-project-id.supabase.co` | Production, Preview, Development |
| `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` | `your-anon-key` | Production, Preview, Development |
| `SUPABASE_SERVICE_ROLE_KEY` | `your-service-role-key` | Production (선택) |

⚠️ **중요 체크사항:**
- `NEXT_PUBLIC_SITE_URL`에 **마지막 슬래시 없이** 입력
  - ✅ 올바른 예: `https://launch-kit-gold.vercel.app`
  - ❌ 잘못된 예: `https://launch-kit-gold.vercel.app/`
- 모든 환경(Production, Preview, Development)에 동일하게 설정

### 3. 재배포

환경 변수 추가 후 **반드시 재배포** 필요:

1. **Deployments** 탭으로 이동
2. 최신 배포 옆 **...** 메뉴 클릭
3. **Redeploy** 선택
4. **Redeploy** 버튼 클릭

---

## Supabase Dashboard 설정

환경 변수 설정 후 Supabase에서도 URL 허용 목록을 업데이트해야 합니다.

### 1. Authentication → URL Configuration

Supabase Dashboard에서:

1. **Authentication** → **URL Configuration** 메뉴로 이동
2. 다음 URL들을 설정:

#### Site URL
프로덕션 도메인 입력:
```
https://launch-kit-gold.vercel.app
```

#### Redirect URLs
이메일 인증 및 OAuth 콜백 URL 추가 (줄바꿈으로 구분):
```
http://localhost:3000/auth/callback
https://launch-kit-gold.vercel.app/auth/callback
```

#### Additional Redirect URLs (선택)
Preview 브랜치 URL이 있다면 추가:
```
https://your-preview-branch.vercel.app/auth/callback
```

### 2. 설정 저장

**Save** 버튼을 클릭하여 변경 사항 저장

---

## 문제 해결

### 문제 1: 이메일 인증 후 랜딩 페이지로 리다이렉트됨

**증상:** 이메일 링크 클릭 후 `/dashboard`가 아닌 `/`(루트)로 이동

**원인:**
- Vercel 환경 변수에 `NEXT_PUBLIC_SITE_URL`이 설정되지 않음
- 환경 변수가 슬래시(`/`)로 끝남
- 환경 변수 추가 후 재배포하지 않음

**해결 방법:**
1. Vercel Environment Variables에서 `NEXT_PUBLIC_SITE_URL` 확인
   - 값: `https://launch-kit-gold.vercel.app` (슬래시 없이)
2. 재배포 (Deployments → ... → Redeploy)
3. Supabase Redirect URLs에 콜백 URL 추가 확인

### 문제 2: OAuth 로그인 실패

**증상:** Google 로그인 후 "redirect_uri_mismatch" 에러

**원인:**
- Supabase Redirect URLs에 콜백 URL이 등록되지 않음
- Google Cloud Console의 승인된 리디렉션 URI와 불일치

**해결 방법:**
1. Supabase → Authentication → URL Configuration → Redirect URLs 확인
2. Google Cloud Console → OAuth 2.0 클라이언트 ID → 승인된 리디렉션 URI 확인
3. 두 설정이 일치하는지 확인

### 문제 3: 환경 변수가 적용되지 않음

**증상:** 코드에서 `process.env.NEXT_PUBLIC_SITE_URL`이 `undefined`

**원인:**
- 환경 변수명 오타 (대소문자 구분)
- 로컬: `.env.local` 파일 저장 후 서버 재시작 안 함
- Vercel: 환경 변수 추가 후 재배포 안 함

**해결 방법:**
1. 환경 변수명 정확히 확인 (`NEXT_PUBLIC_SITE_URL`)
2. 로컬: 개발 서버 재시작 (`yarn dev`)
3. Vercel: 재배포 수행

### 문제 4: Preview 브랜치에서 인증 실패

**증상:** Preview 배포에서 이메일 인증/OAuth 실패

**원인:**
- Preview 브랜치 URL이 Supabase Redirect URLs에 등록되지 않음

**해결 방법:**
1. Preview 브랜치 URL 확인 (예: `https://launch-kit-abc123.vercel.app`)
2. Supabase → Redirect URLs에 추가:
   ```
   https://launch-kit-abc123.vercel.app/auth/callback
   ```
3. 또는 와일드카드 사용 (덜 안전):
   ```
   https://*.vercel.app/auth/callback
   ```

---

## 체크리스트

배포 전 환경 변수 설정 확인:

### 로컬 개발
- [ ] `.env.local` 파일 생성
- [ ] `NEXT_PUBLIC_SITE_URL=http://localhost:3000` 설정
- [ ] `NEXT_PUBLIC_SUPABASE_URL` 설정
- [ ] `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` 설정
- [ ] `.env.local`이 `.gitignore`에 포함되어 있는지 확인
- [ ] 개발 서버 재시작

### Vercel 프로덕션
- [ ] Vercel Environment Variables에 `NEXT_PUBLIC_SITE_URL` 추가 (슬래시 없이)
- [ ] Vercel Environment Variables에 `NEXT_PUBLIC_SUPABASE_URL` 추가
- [ ] Vercel Environment Variables에 `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` 추가
- [ ] 모든 환경(Production, Preview, Development)에 동일하게 설정
- [ ] 환경 변수 추가 후 재배포

### Supabase Dashboard
- [ ] Authentication → URL Configuration → Site URL 설정
- [ ] Redirect URLs에 로컬 콜백 URL 추가 (`http://localhost:3000/auth/callback`)
- [ ] Redirect URLs에 프로덕션 콜백 URL 추가 (`https://your-domain.com/auth/callback`)
- [ ] 설정 저장

### 테스트
- [ ] 로컬: 이메일 회원가입 → 이메일 링크 클릭 → 대시보드 진입 확인
- [ ] 로컬: Google 로그인 → 대시보드 진입 확인
- [ ] 프로덕션: 이메일 회원가입 → 이메일 링크 클릭 → 대시보드 진입 확인
- [ ] 프로덕션: Google 로그인 → 대시보드 진입 확인

---

## 참고 문서

- [Supabase Auth 공식 문서](https://supabase.com/docs/guides/auth)
- [Next.js 환경 변수 가이드](https://nextjs.org/docs/basic-features/environment-variables)
- [Vercel 환경 변수 설정](https://vercel.com/docs/concepts/projects/environment-variables)
- [OAuth 설정 가이드](./oauth-setup.md)
