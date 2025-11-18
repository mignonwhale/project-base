# Next.js 프로젝트 생성 Troubleshooting

## 발생한 오류 및 해결방법

### 1. shadcn/ui 초기화 시 색상 선택 이슈

**오류 상황:**
- `yarn dlx shadcn@latest init` 실행 시 대화형 프롬프트에서 색상 선택이 자동화되지 않음
- 기본적으로 Neutral이 선택되어 문서에서 지정한 Zinc 색상이 선택되지 않음

**해결방법:**
- 대화형 프롬프트에서 자동으로 Zinc를 선택하는 방법을 시도했으나 실제로는 Neutral이 선택됨
- 실제 프로젝트에서는 수동으로 Zinc를 선택하거나, components.json 파일을 직접 수정하여 해결 가능
- 또는 shadcn init 시 --defaults 옵션을 사용하여 기본값으로 설정 후 수정

### 2. 개발 서버 종료 명령어 이슈

**오류 상황:**
- Windows 환경에서 `pkill` 명령어가 존재하지 않음
- `taskkill` 명령어 사용 시 파라미터 오류 발생

**해결방법:**
- Windows에서는 `Ctrl+C`를 통해 개발 서버를 직접 종료하는 것이 안전
- 백그라운드 프로세스 종료가 필요한 경우 작업 관리자 사용 권장

### 3. Deprecation Warning 발생

**오류 상황:**
```
(node:4648) [DEP0169] DeprecationWarning: `url.parse()` behavior is not standardized
(node:4648) [DEP0190] DeprecationWarning: Passing args to a child process with shell option true can lead to security vulnerabilities
```

**해결방법:**
- 이는 Next.js 및 관련 패키지의 내부적인 deprecated API 사용으로 발생
- 실제 애플리케이션 동작에는 영향 없음
- 향후 Next.js 업데이트를 통해 자동 해결될 예정

### 4. globals.css 초기화 시 shadcn 스타일 제거

**오류 상황:**
- shadcn/ui 초기화 후 globals.css에 많은 CSS 변수와 스타일이 추가됨
- 문서에서는 `@import "tailwindcss";`만 남기도록 지시

**해결방법:**
- 실제 프로젝트에서는 shadcn/ui 컴포넌트를 사용할 경우 해당 스타일들이 필요함
- 순수한 TailwindCSS만 사용하려는 경우에만 제거 권장
- shadcn/ui 컴포넌트 사용 시에는 자동 생성된 스타일 유지 필요

## 권장사항

### 1. 프로덕션 환경 고려사항
- shadcn/ui 컴포넌트를 사용할 계획이라면 globals.css의 CSS 변수들을 유지
- 프로젝트 초기 설정 시 어떤 UI 라이브러리를 사용할지 미리 결정

### 2. 개발 환경 설정
- Windows 환경에서는 WSL 사용을 고려하여 Linux 명령어 호환성 확보
- Git Bash 또는 PowerShell Core 사용 권장

### 3. 버전 관리
- yarn 4.9.2로 업그레이드 후 .yarnrc.yml 파일이 자동 생성됨
- 이 파일도 버전 관리에 포함시켜야 팀원들과 동일한 환경 유지 가능