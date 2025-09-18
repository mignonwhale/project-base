## 클로드코드 세팅
1. 클로드코드 로컬 세팅
```
npm install -g @anthropic-ai/claude-code
```

2. intellij claude code plugin 설치

3. 터미널 세팅
- 줄바꿈
```
/terminal-setup
```


# CLAUDE.md 예시
## 기술 스택
- Framework: Next.js 15.5.2 (App Router)
- React 19.1.1
- 언어: TypeScript 5.8.3
- 상태관리: 
- 스타일링: Tailwind CSS 3.4.1
- 디자인 시스템
- 테스팅: 
- 패키지 매니저: Yarn 4.2.2
- 프로젝트 버전: 


## 개발 규칙
### 1. 코드 작성 규칙
- 절대 모킹하지 않기: 실제 동작하는 코드만 작성
- 타입 안정성: TypeScript 엄격 모드 준수
- 테스트 우선: 테스트 커버리지 90% 이상 유지
- 컴포넌트 네이밍: PascalCase, 기능을 명확히 나타내는 이름 사용
- React import: React 타입 사용 시 named import 사용 (`import {type ReactNode, type FC} from 'react';`)

### 2. 패캐지 버전 호환성
- React 19.1.1 고정(resolutions 설정됨)
- 새 캐키지 추가 시 기존 의존성과 충돌 확인 

### 3. 파일 구조 규칙
- `index.ts`로 export 모듈화
- `.unit.spec.tsx` 확장자로 단위 테스트 작성
- `.types.ts`, `.constants.ts`, `.utils.ts` 분리

### 4. 스크립트 명령어
```bash
yarn dev            # 개발서버
yarn build          # 프로덕션 빌드
yarn lint           # ESLint 검사
yarn test:unit      # Jest 단위 테스트
yarn test:e2e       # Cypress E2E 테스트
```


# 클로드코드랑 같이 작성하기 프롬프트
```
현재 프로젝트를 분석해서 CLAUDE.md 만들어줘, package.json, tsconfig, 주요 컴포넌트를 참고해서 기술 스택, 코딩 컨벤션, 프로젝트 구조 섹션 포함해줘
```

```
이전 커밋의 변경사항을 확인해서 CLAUDE.md에 추가할 내용이 있으면 제시해줄래?
```
```
CLAUDE.md에서 가장 많이 참조된 규칙과 가장 참조되지 않은 규칙을 알려줘
```

# AI 지침
- 모든 API KEY, TOKEN 등과 같은 중요한 정보는 외부에 노출하지 않는다. 물론 git에도 커밋을 하지 않고 하려는 시도가 있을 경우 꼭 알린다.
- 오류나 에러가 발생한 경우, 원인파악 > 해결방안 > 개발자 확인 > 테스트 > 소스수정의 과정을 거친다. 
    - 한 번에 모든 사항을 말하지 말고 하나의 오류당 위 단계를 거친다. 
    - 소수 수정 후 다른 비슷한 코드나 동일 적용이 필요한 코드가 있으면 개발자 확인 후 수정한다. 
- 계획, 설계서, 체크리스트 등이 최신 상태인지 확인하고 git에 변경 사항을 적용한다.