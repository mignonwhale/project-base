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