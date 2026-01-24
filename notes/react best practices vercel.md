Cursor에 이 레포의 agent-skills를 쓰려면, 기본적으로 **Agent Skills CLI를 전역 설치**해서 프로젝트에 붙이는 방식이 가장 간단합니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​

## 1. 사전 이해

- 이 레포는 `Agent Skills` 포맷으로 만들어진 **스킬 모음집**이라, Claude·기타 에이전트가 쓸 수 있는 “지침 + 스크립트 번들”입니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​
    
- 설치 후에는 에이전트가 코드 리뷰, React/Next 최적화, UI/UX 점검, Vercel 배포 같은 작업에 이 스킬들을 자동으로 활용하게 됩니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​
    

## 2. 기본 설치 (프로젝트 기준)

Cursor에서 작업 중인 프로젝트 루트에서 아래처럼 실행하면 됩니다.

bash

`cd <내_프로젝트_폴더> npx add-skill vercel-labs/agent-skills`

- 이 커맨드는 `vercel-labs/agent-skills`에 정의된 여러 스킬들을 **한 번에 프로젝트에 추가**합니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​
    
- 이후 에이전트(예: Claude, OpenAI 기반 등)가 “React 성능 최적화 해줘”, “UI 접근성 리뷰해줘” 같은 요청을 받을 때 이 스킬 지식을 참고하게 됩니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​
    

## 3. Cursor에서 실제 사용 패턴

설치 후에는 Cursor 안에서 평소처럼 프롬프트만 잘 써도 됩니다.

- React 성능: “이 컴포넌트 React best practice 기준으로 성능 리뷰해줘.”
    
- UI/UX: “이 페이지 UI를 web-design-guidelines 기준으로 접근성/UX 리뷰해줘.”
    
- 배포: “이 프로젝트를 Vercel에 배포해줘. claim 가능한 링크도 줘.”
    

이런 식의 자연어 요청을 하면, `react-best-practices`, `web-design-guidelines`, `vercel-deploy-claimable` 같은 스킬이 트리거되는 형태입니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​

## 4. 스킬 구조 이해 (필요 시 커스터마이즈)

설치된 스킬들은 대략 이런 구조를 가집니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​

- `SKILL.md`: 에이전트가 따라야 할 상세 지침.
    
- `scripts/`: 필요하면 실행하는 헬퍼 스크립트.
    
- `references/`: 참고용 문서.
    

Cursor에서 이 파일들을 열어보면서 **자신의 팀 규칙에 맞게 SKILL.md를 수정**하면, 팀 스타일에 맞는 “커스텀 코드 리뷰/가이드라인 봇”처럼 쓸 수 있습니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​

## 5. 만약 전역/다른 프로젝트에도 쓰고 싶다면

- 전 프로젝트 공통으로 쓰고 싶다면, **템플릿 리포** 하나에 위 커맨드를 실행한 뒤 그 템플릿을 복제하는 방식으로 쓰는 게 관리가 편합니다.
    
- 또는 각 프로젝트에서 필요할 때마다 `npx add-skill vercel-labs/agent-skills`를 실행해도 됩니다.[[github](https://github.com/vercel-labs/agent-skills?tab=readme-ov-file)]​
    

원하면: Cursor에서 지금 쓰는 에이전트 종류(Claude/OpenAI 등)랑, 어떤 작업(코드 리뷰, 배포, 디자인 리뷰 등)에 쓰고 싶은지 알려주면 거기에 맞는 프롬프트 예시도 정리해 줄 수 있어요.