# Next.js 프로젝트 생성

## 개발환경 

- `Next.js`
- `Typescript`
- `yarn`


## 환경세팅

### 프로젝트 세팅
- `package.json`, `yarn.lock`, `node_modules` 자동 생성
```
yarn create next-app your-project-name --typescript
```

## yarn 로컬 업그레이드
- `yarn` 버전 올림 
```
  "packageManager": "yarn@4.9.2"
```

- `yarn` 실행: 자동으로 , `.yarnrc.yml` 생성됨.
```
yarn
```

## WebStorm의 yarn 설정
Preferences > Languages & Frameworks > Node.js에서 Package Manager 항목을 yarn으로 변경.

## CSS 세팅
- 프로젝트 세팅 시 tailwind css 설치를 선택하면 자동으로 세팅된다.
- Tailwind CSS v4+일 경우 설정파일이 없어도 기본적으로 css 적용이 된다.
- 커스텀 설정이 필요한 경우에는 `tailwind.config.ts`를 추가하면 된다. 

```package.json
    "@tailwindcss/postcss": "^4",
    "tailwindcss": "^4",
```

```global.css
@import "tailwindcss";
```

## shadcn 사용 시
```bash
# CLI 설치 (개발 의존성, 배포 안함)
yarn add -D shadcn-ui 

# 초기화 → components.json 생성
yarn shadcn init 

# 원하는 UI 컴포넌트를 가져와서 components/ 폴더에 추가
yarn shadcn add button 

## Usage Error: Couldn't find a script named "shadcn". 에러가 나는 경우 아래 명령어로 
yarn dlx shadcn@latest add button
```



## vercel 세팅
- [vercel](https://vercel.com/)에서 프로젝트 생성 후 gitHub repository만 연결하면 자동 배포 실행


1. Vercel 계정 생성/로그인
  - https://vercel.com 접속
  - GitHub 계정으로 로그인
2. 새 프로젝트 생성
  - "New Project" 클릭
  - GitHub repository 연결
  - [project] 저장소 선택
3. 프로젝트 설정
  - Framework Preset: Next.js
  - Root Directory: ./
  - Build Command: yarn build
  - Output Directory: .next (자동 감지)
  - Install Command: yarn install

4. 환경변수 설정
  - "Environment Variables" 섹션에서 다음 추가:

● 환경변수 설정 (Vercel 대시보드):
NEXT_PUBLIC_SUPABASE_URL = [YOUR_SUPABASE_PROJECT_URL]
NEXT_PUBLIC_SUPABASE_ANON_KEY = [YOUR_SUPABASE_ANON_KEY]

5. 배포 실행
  - "Deploy" 버튼 클릭
  - 자동 빌드 및 배포 진행
