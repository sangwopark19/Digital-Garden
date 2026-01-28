이 글은 Cursor 에이전트로 코드를 짤 때 생산성을 극대화하는 **워크플로우 베스트 프랙티스**를 정리한 가이드입니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​

## 핵심 개요

- 에이전트는 여러 파일을 수정하고 테스트를 돌리며 긴 시간 작업할 수 있고, 이를 잘 활용하려면 **계획–컨텍스트 관리–룰·스킬 확장–리뷰** 같은 패턴을 이해해야 한다고 설명합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- Cursor 팀이 내부 평가를 통해 모델별로 **명령어, 도구 사용 방식, 시스템 프롬프트**를 튜닝해 두었으므로 사용자는 워크플로우에 집중하면 된다고 강조합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## 계획 세우기와 Plan Mode

- 가장 큰 레버리지로 **“코딩 전에 계획을 세우는 것”**을 꼽고, Plan Mode에서 코드베이스 리서치 → 요구사항 질문 → 파일·코드 참조가 포함된 구현 계획 생성 → 승인을 기다리는 흐름을 제공합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- Plan 파일은 마크다운으로 열려 불필요한 단계 삭제, 접근 방식 수정, 추가 컨텍스트 입력 후 다시 실행할 수 있고, `.cursor/plans/`에 저장해 팀 문서와 재사용 가능한 컨텍스트로 활용할 수 있습니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## 컨텍스트 관리와 대화 전략

- 파일을 전부 태그할 필요 없이 에이전트가 `grep`과 시맨틱 검색으로 **관련 파일을 스스로 찾게 두고**, 정말 필요한 파일만 명시하라고 권장합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- 작업이 바뀌거나 에이전트가 계속 헷갈리면 **새 대화로 시작**하고, 이전 작업은 `@Past Chats`로 참조하라고 안내합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## Rules·Skills·Hooks로 에이전트 확장

- `.cursor/rules/`의 마크다운으로 **항상 적용되는 규칙(명령어, 코드 스타일, 워크플로우 패턴)**을 정의하되, 전체 스타일 가이드 복붙은 피하고 핵심만 짧게 유지하라고 합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- `SKILL.md`로 정의하는 Skills는 `/` 명령, 훅(hook), 도메인 지식 등을 포함해 필요할 때만 동적으로 불러오며, 예시로 테스트가 전부 통과할 때까지 반복 작업시키는 **long-running loop hook** 패턴을 보여줍니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## 테스트, 코드 이해, Git·리뷰 워크플로우

- TDD 패턴으로 **테스트 먼저 작성 → 실패 확인 → 테스트 커밋 → 구현 작성·반복 → 구현 커밋**을 에이전트에게 맡기는 단계를 제안합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- 새 코드베이스 온보딩 시에는 “로그 구조는?”, “새 API 엔드포인트 추가 방법은?”처럼 동료에게 묻듯 질문해 에이전트가 코드 검색·이해를 대신하게 하라고 설명합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- `.cursor/commands/`에 `/pr`, `/fix-issue`, `/review`, `/update-deps` 같은 Git·리뷰용 커맨드를 저장해, 다단계 작업을 한 번에 위임하는 패턴도 소개합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## 병렬 에이전트, 클라우드 에이전트, 디버깅

- Git worktree를 자동으로 관리하여 **여러 에이전트를 병렬로 서로 다른 워크트리에 띄워** 충돌 없이 작업하고, 여러 모델을 동시에 돌려 결과를 비교·선택하는 패턴을 지원합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- 클라우드 에이전트는 버그 수정, 리팩터, 테스트 생성, 문서 갱신 등 TODO성 작업을 **원격 샌드박스에서 브랜치 생성→작업→PR 생성**까지 맡기는 용도로 설명합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    
- Debug Mode는 가설 생성→로그 주입→재현 중 런타임 데이터 수집→원인 분석→타깃 수정의 절차로, 재현 가능한 버그·레이스 컨디션·성능 문제에 특히 유용하다고 합니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​
    

## 마지막 조언

- 이 글은 에이전트를 잘 쓰는 개발자의 공통점으로 **구체적인 프롬프트 작성, 설정·Rules·Commands의 점진적 개선, 꼼꼼한 리뷰, 테스트·타입·린터 같은 검증 신호 제공, 에이전트를 동료처럼 대하기**를 듭니다.[[cursor](https://cursor.com/blog/agent-best-practices)]​