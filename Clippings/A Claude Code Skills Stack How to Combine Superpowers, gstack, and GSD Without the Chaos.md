---
title: "A Claude Code Skills Stack: How to Combine Superpowers, gstack, and GSD Without the Chaos"
source: "https://dev.to/imaginex/a-claude-code-skills-stack-how-to-combine-superpowers-gstack-and-gsd-without-the-chaos-44b3"
author:
  - "[[Yaohua Chen]]"
published: 2026-04-07
created: 2026-04-15
description: "One article to compare the frameworks, see where they overlap, and land on a stable three-layer... Tagged with ai, sde, claude."
tags:
  - "clippings"
---
> 프레임워크를 비교하고 겹치는 부분을 파악한 후 안정적인 3계층 실천 방안에 도달하는 한 편의 글입니다.

## 소개

Claude Code는 빠르게 가장 널리 채택된 AI 코딩 도구 중 하나가 되었습니다. 개별 개발자, 스타트업, 대규모 엔지니어링 팀 모두 이를 일상 워크플로우에 통합했으며, 프로덕션 코드 작성, 풀 리퀘스트 검토, 디버깅, 기능 배포를 1년 전에는 상상하기 어려웠던 속도로 진행하고 있습니다. 사용량이 증가함에 따라 이를 둘러싼 생태계도 함께 성장했습니다. **Claude Skills** —에이전트가 계획하고, 구축하고, 검증하는 방식을 형성하는 조합 가능한 자동 호출 명령어 집합—은 Claude Code의 가장 중요한 확장 포인트 중 하나로 부상했습니다. 이를 통해 일회성 프롬프트를 넘어 **반복 가능한 워크플로우** 를 에이전트의 동작에 직접 인코딩할 수 있습니다. 실제로 Anthropic은 이 방향을 강화했습니다. 최신 버전의 Claude Code는 **이전의 분리된 "슬래시 명령어"와 "스킬" 시스템을 단일의 통합된 스킬 형식으로 통합** 했으며, 이는 스킬이 이제 에이전트를 확장하는 표준적인 방식임을 나타냅니다.

Skills가 이제 경험의 중심이 되면서 커뮤니티는 모범 사례를 기성 스킬 세트로 패키징한 몇 가지 오픈소스 프레임워크를 중심으로 결집했습니다. 가장 많이 논의되는 두 스택은 **Superpowers** 와 **gstack** 입니다. 둘 다 설치하는 것은 쉬워 보이지만, 실제로는 **충돌** 할 수 있으며, 계획 없이 프레임워크를 쌓아올리면 설정이 더 안정적이 되는 것이 아니라 **덜** 안정적이 되는 경우가 많습니다. 그렇다면 둘의 차이점은 무엇이며, 어떻게 선택해야 할까요?

이 글은 세 가지를 다룹니다:

1. **Superpowers와 gstack을 저장소, 기능, 철학 측면에서 비교** 합니다—아래의 스타 수, 스킬 목록, 트레이드오프에 관한 자료를 포함합니다.
2. **많은 가이드에서 건너뛰는 세 번째 레이어를 추가** 합니다: **GSD** 를 **컨텍스트 / 스펙** 안정화 도구로 활용하여 장기 작업이 표류하지 않도록 합니다 (*Tricia Notes Editorial의 3레이어 프레임워크에서 영감을 받음*).
3. **단일 플레이북으로 마무리** 합니다: **의사결정**, **컨텍스트**, **실행** 을 누가 담당하는지, 그리고 토큰 사용량이나 인지 부하를 증가시키지 않으면서 스킬을 선택적으로 활용하는 방법을 제시합니다.

유용한 질문은 단순히 "Superpowers **또는** gstack?" 이 아니라: *당신이 놓치고 있는 것이 무엇인가—의사결정, 지속 가능한 컨텍스트, 아니면 실행인가?*

**한 줄로 요약하면:** *gstack은 생각하고, GSD는 안정화하며, Superpowers는 실행한다.*

---

## 방향성: 두 개가 아닌 세 개의 레이어

실제로 안정적으로 유지되는 것은 종종 한 프레임워크를 다른 것보다 선택하는 것이 **아니라**, **세 가지 역할 분담** 이다.

| 레이어 | 스택 | 역할 |
| --- | --- | --- |
| **의사결정 / 역할** | **gstack** | CEO, 디자인, 아키텍처, QA 관점에서의 판단—단순히 "코드 작성 방법"만이 아닌. |
| **Context / spec** | **GSD** | 스펙, 상태, 경계, 그리고 장기적 컨텍스트가 부패하지 않도록 유지. |
| **실행** | **초능력** | 요구사항 명확화 → 계획 → TDD → 수용을 **폐쇄 루프** 로. |

**각각이 "강력한" 이유:**

1. **Superpowers** — **어떻게** 작업이 완료되는지; 매끄러운 실행 루프.
2. **gstack** — **무엇을** 할 것인지와 **그것이** 수행되어야 하는지 여부; 더 풍부한 역할 기반 판단.
3. **GSD** — **표류하지 않기**; 긴 체인에서 더 안정적인 스펙과 컨텍스트.

Superpowers와 gstack 모두 바이럴 현상을 일으켰습니다. 표면적으로는 AI에 프로세스를 추가하지만, 실제 사용에서는 **무엇이 중요한지 명확하게 생각하도록** 도와줍니다. 모델이 빠르게 코딩할 때, 정확히 그때가 명확한 요구사항과 안정적인 컨텍스트가 필요한 순간입니다— **이것이** 대부분의 사람들이 여전히 간과하는 부분입니다.

---

## Superpowers vs gstack: 핵심 정보

### Superpowers (GitHub ~137K stars)

- 저장소: **obra/superpowers**
- **Agent Skills** 프레임워크 및 소프트웨어 개발 방법론: 브레인스토밍, 계획, TDD, 실행, 검증에 걸친 **14개의 내장 스킬**.

### gstack (GitHub ~65K 스타)

- 저장소: **garrytan/gstack**
- **YC CEO Garry Tan** 의 오픈소스.
- 철학: 당신 곁의 **팀** —CEO, 디자이너, 엔지니어링 매니저, 릴리스 매니저, 문서 엔지니어, QA 등— **23개의 의견이 반영된 도구** (제품 사고, CEO 리뷰, 아키텍처 리뷰, 실제 브라우저 테스팅, 디자인 리뷰, 보안 감사 등).
- Garry는 YC를 풀타임으로 운영하면서 부분적으로 **60일 동안 600K+ 줄의 프로덕션 코드(35% 테스트)를 작성** 했다고 주장했습니다.

스타는 약한 지표입니다: 높은 스타 수가 모든 스킬이 당신의 워크플로우에 맞다는 의미는 아닙니다.

---

## 기능 비교 (Superpowers vs gstack)

| 카테고리 | Superpowers | gstack |
| --- | --- | --- |
| **제품 브레인스토밍** | 브레인스토밍 | /office-hours, /plan-ceo-review |
| **아키텍처 계획** | 작성 계획 | /plan-eng-review, /autoplan |
| **디자인** | — | /design-consultation, /plan-design-review, /design-shotgun, /design-html |
| **개발 실행** | 계획 실행, 서브에이전트 기반 개발, 병렬 에이전트 디스패칭 | — |
| **테스팅** | 테스트 주도 개발 | /qa, /qa-only |
| **디버깅** | systematic-debugging | /investigate |
| **코드 리뷰** | 코드 리뷰 요청, 코드 리뷰 수신 | /review, /codex |
| **검증 및 수락** | 완료 전 검증, 개발 브랜치 완료 | /ship, /land-and-deploy, /canary, /document-release |
| **보안** | — | /cso, /careful, /freeze, /guard, /unfreeze |
| **관찰성** | — | /learn, /retro |
| **브라우저 테스팅** | — | /browse, /connect-chrome, /setup-browser-cookies |
| **Git 워크트리** | git-worktrees 사용하기 | — |
| **스킬 관리** | using-superpowers, writing-skills | /gstack-upgrade |
| **성능** | — | /benchmark |
| **배포** | — | /setup-deploy |

커버리지는 많이 다릅니다; **양이 핵심이 아니라 설계 철학입니다.**

---

## 설계 철학: "How" vs "What" (그리고 GSD가 어디에 맞는지)

### Superpowers — 코드가 어떻게 빌드되는지에 초점

워크플로우는 **고품질 산출물** 을 중심으로 합니다: 명확히 하기, 계획 수립, **TDD** (구현 전 테스트), 검증. 각 단계마다 체크포인트가 있어서 건너뛸 여지가 거의 없습니다. 실제로는 **규율 있는** 느낌입니다: X를 요청하면 X를 구축하는 경향이 있습니다. 이미 무엇을 구축할지 알고 있는 엔지니어들은 이를 종종 강력하다고 느낍니다.

*(실행 계층 세부사항 실제 사용 경험에서: 강력한 프로세스와 안정적인 실행; 작은 작업도 여전히 \*\*무거울 수 있습니다* \* 왜냐하면 전체 리듬이 작은 요청에도 적용되기 때문입니다.)\*

### gstack — 무엇을 해야 하고 무엇을 하지 말아야 하는지에 집중

본격적인 코딩 전에 **/office-hours** 와 같은 플로우가 요구사항을 검토하고, **CEO** 와 **엔지니어링** 리뷰가 접근 방식을 스트레스 테스트합니다. 단순한 코드만이 아니라 사용자 관점에서 **실제 브라우저 테스트를 실행** 할 수 있습니다. 대략적인 분류:

- **의사결정 레이어:** `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`
- **실행 레이어:** `/review`, `/qa`, `/ship` 등

gstack은 요구사항이 여전히 모호할 때 빛을 발합니다—PM, 인디 개발자, 또는 "빌드하면서 생각하기" 방식에 최적입니다. 주의: 모든 역할을 켜면 **과도해** 보일 수 있으며, 의사결정 스킬도 상당한 토큰을 소비합니다(아래 참조).

### GSD — 컨텍스트 / 스펙, 또 다른 "팀 차트"가 아님

**GSD** 는 "또 다른 팀을 설치하는 것"이 아닙니다. 이것은 **컨텍스트 엔지니어링** 입니다: 목표, 사양, 상태, 경계, 그리고 요약이 고정되어 **컨텍스트 부패** 가 느려집니다. 짧은 데모는 이를 숨기지만, **긴** 프로젝트에서는 드러납니다—컨텍스트가 흔들릴 때, 결과물이 산재됩니다; 이것은 단순한 "나쁜 실행"이 아니라 **상태** 입니다.

- **gstack** 은 생각하지만, 그 자체로는 장기 컨텍스트 저장소가 아닙니다.
- **Superpowers** 는 실행하지만, 그 자체로는 스펙/컨텍스트 시스템이 아닙니다.
- **GSD** 는 체인이 일관성 있게 유지되도록 그 간격을 채웁니다.

---

## 3가지 방식 비교 (승자가 아닌 문제점들)

| 차원 | Superpowers | gstack | GSD |
| --- | --- | --- | --- |
| **핵심 질문** | 일을 완료하는 방법 | 무엇을 할 것인가; 그것이 해야 할 일인지 여부 | 프로젝트가 산으로 가지 않도록 하는 방법 |
| **레이어** | 실행 | 의사결정 / 역할 | 배경 / 사양 |
| **가장 강력한 적합성** | 계획, TDD, 수용 루프 | 다중 관점 판단, 리뷰, QA | 컨텍스트 엔지니어링; 안정적 상태 |
| **최적의 용도** | 명확한 요구사항 | 구축하면서 생각하기 | 긴 체인 / 많은 반복 |
| **일반적인 문제점** | 사전에 로드된 프로세스는 무거울 수 있습니다 (아래 세부사항 참조) | 완전히 활성화되면 비대하고 토큰을 많이 소비합니다 (아래 세부사항 참조) | 자체적으로는 거의 독립적인 "배포" 가치가 없습니다 (아래 세부사항 참조) |
| **역할** | 자신의 **실행** | 자신의 **의사결정** | 자신의 **장기 컨텍스트** |

### 세부 공통 문제점

**Superpowers — 사전 로딩된 프로세스가 무거울 수 있습니다.** 모든 작업은 크기에 관계없이 전체 사이클을 거칩니다: 요구사항 명확화, 계획 수립, 테스트 우선 작성, 구현, 검증. 대규모 기능의 경우 이러한 리듬이 큰 효과를 발휘합니다. 두 줄의 설정 수정이나 빠른 복사 변경의 경우, 동일한 절차가 적용되어 실제 변경보다 프로세스에 더 많은 시간을 소비하게 됩니다. 오버헤드가 작업 크기에 따라 축소되지 않으므로, 작은 요청은 불균형하게 느려질 수 있습니다.

**gstack — 완전히 활성화되면 비대하고 토큰을 많이 소비합니다.** 각 gstack 역할(CEO, 디자이너, 아키텍트, QA 등)은 자신의 관점과 프롬프트를 컨텍스트에 주입합니다. 모두 켜면 단일 실행 레이어 스킬만으로도 실제 코드가 작성되기 전에 **10K+ 토큰** 을 소비할 수 있습니다. 일일 사용량은 토큰을 빠르게 소진하며, 여러 "가상 팀 멤버" 간의 왕복은 간단한 작업도 느리고 중복되게 느껴질 수 있습니다. 또한 코드베이스를 스캔하는 동안 관련 없는 메타 질문(예: "YC 회사가 되기 위해 지원하고 있습니까?")을 마주칠 수 있습니다. 이는 프레임워크의 독단적인 페르소나 레이어의 산물입니다.

**GSD — 작은 독립형 "배포" 가치.** GSD는 스펙, 목표, 상태를 긴 세션 동안 고정된 상태로 유지하는 데 탁월합니다. 하지만 **단독으로** 사용하면 직접적으로 코드를 생성하거나, 테스트를 실행하거나, PR을 열지 못합니다. 이는 빌더가 아닌 안정화 도구입니다. 실행 레이어(Superpowers)나 의사결정 레이어(gstack)가 함께 없으면, GSD는 아무것도 작용하지 않는 컨텍스트를 관리합니다—유용한 기반 시설이지만 눈에 띄는 결과물은 없습니다. 그 가치는 실제로 작업을 배포하는 도구들과 결합될 때만 명확해집니다.

**실무적 교훈:** 이들은 **상호 보완적** 이며 대체 관계가 아닙니다—Superpowers는 실행하고, gstack은 의사결정을 하며, GSD는 시간에 따라 사양과 컨텍스트를 안정화합니다.

---

## 강점, 약점, 그리고 마찰

### Superpowers

- **강점:** 브레인스토밍과 전체 워크플로우가 견고함; 작은 작업이라도 습관화되면 전체 프로세스가 매끄러워질 수 있음; 실행과 TDD가 강함.
- **약점:** 약한 부분은 종종 gstack의 의사결정 레이어에 비해 **초기** 의사결정 능력(예: 계획/브레인스토밍)이 약하다는 점입니다—따라서 많은 사람들이 gstack의 프론트엔드와 Superpowers의 실행을 **조합** 하여 사용합니다.

### gstack

- **강점:** **의사결정 레이어** — `/office-hours`, `/plan-ceo-review`, `/plan-eng-review` —포지셔닝과 접근 방식 검토에서 두각을 나타냅니다.
- **약점:** 실행이 Superpowers에 비해 거칠게 느껴집니다. 토큰 비용이 실질적입니다— **단일 실행 레이어 스킬 하나가 10K+ 토큰을 소비할 수 있으며**, 무거운 스캔은 도움이 아닌 시끄러운 "프로세스"처럼 느껴질 수 있습니다.

### 비유

> **Superpowers는 메스다** — 정확하고 효율적이다.  
>   
> **gstack는 완전한 클리닉** — 진단부터 사후관리까지.

은유를 사용하여 깊이를 선택하세요: 좁은 실행 vs 전체 스펙트럼 제품 및 검토.

---

## 통합된 모범 사례

### 1\. 스킬을 의도적으로 선택하세요 — 모든 것을 설치하지 마세요

스킬 개수는 쉽게 증가합니다(오늘은 Superpowers, 내일은 gstack, 다음 주는 또 다른 스택). **선택적 배포** 가 양보다 낫습니다. 무작위 호출은 불안정해 보이며 명확성 없이 표면적인 "스킬 개수"만 부풀립니다.

기본 개념: 두 스택 모두 **Harness Engineering** 의 실험입니다. 마인드셋은 **강점을 활용하고, 약점을 보완한다** 는 것입니다—"모든 것을 원한다"가 아니라.

### 2\. 의사결정 대 실행(고전적인 분리)—필요할 때 컨텍스트 추가

**의사결정 레이어를 위한 gstack(선별된 항목):**

- 고가치 플로우 우선순위 지정: 예를 들어 `/office-hours`, `/plan-ceo-review`, `/plan-eng-review` 는 요구사항과 정렬을 위해 사용하되, 모든 역할에 과도하게 투자하는 것을 피합니다.

**실행 레이어를 위한 Superpowers:**

- Superpowers를 TDD, 계획-실행, 검증의 **기반** 으로 선호하되, gstack이 이미 의사결정 단계를 담당하고 있다면 Superpowers의 무거운 의사결정 기능을 **약화** 시켜 소규모 작업이 이중 프로세스를 상속받지 않도록 하세요.

**체인이 분기할 때의 GSD:**

- 작업이 세션과 스레드에 걸쳐 **분산** 되면, 스펙과 상태가 고정된 상태로 유지되도록 **GSD** 를 추가하세요—화려함을 위한 것이 아니라, **드리프트 방지** 를 위해서입니다.

### 3\. 안정적인 워크플로우 (3단계)

1. **의사결정 → gstack** — `/office-hours` 로 아이디어를 스트레스 테스트한 후, `/plan-ceo-review` 를 실행하여 창업자 수준의 정합성 검증을 하고 `/plan-eng-review` 로 아키텍처와 데이터 흐름을 확정합니다. 디자인이 중요하면 `/plan-design-review` 를 추가합니다. 목표: 코드를 건드리기 전에 \*\*무엇을\*\* 구축할지, \*\*구축해야 할지\*\* 결정하기.
2. **배경 → GSD** — 결정이 내려지면 GSD(v2)를 사용하여 계획을 구체화하세요: `PROJECT.md` 에는 프로젝트의 목적을, `DECISIONS.md` 에는 아키텍처 선택 사항을, `KNOWLEDGE.md` 에는 세션 간 규칙과 패턴을, 그리고 `M001-ROADMAP.md` 와 같은 마일스톤 로드맵에는 단계별 실행 계획을 기록합니다. 이러한 v2 아티팩트는 사양, 상태 및 경계를 안정적으로 유지하여 세션 간에 컨텍스트가 훼손되지 않도록 합니다. (원래 GSD는 대신 `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md` 를 사용합니다.)
3. **실행 → Superpowers** — 명확한 요구사항과 안정적인 컨텍스트가 준비되면, Superpowers의 실행 루프로 넘깁니다: `brainstorming` (경량 개선이 여전히 필요한 경우), `writing-plans` → `executing-plans` (구현용), `test-driven-development` (RED-GREEN-REFACTOR 사이클용), `requesting-code-review` / `receiving-code-review` (리뷰용), 그리고 `verification-before-completion` → `finishing-a-development-branch` (루프 종료용). 병렬 작업의 경우 `dispatching-parallel-agents` 또는 `subagent-driven-development` 를 사용합니다.

**통합된 태그라인:** *gstack은 사고를 담당하고, Superpowers는 실행을 담당하며, GSD는 긴 컨텍스트를 정직하게 유지합니다.* gstack의 **강력한 의사결정 슬라이스** 와 **Superpowers의 실행** (필요시 GSD 포함)을 결합하면 스킬 개수와 충돌을 제어 상태로 유지할 수 있습니다—저자가 주말에 **엄선된** 조합으로 작은 도구를 구축한 경험과 유사합니다.

### 4\. 최종 휴리스틱

1. 요구사항이 여전히 모호함 → **gstack으로 시작** (의사결정).
2. 작업이 체인 전체에서 계속 분산됨 → **GSD 추가** (컨텍스트).
3. 실행이 **안정적이고 폐쇄 루프** 를 원함 → **Superpowers 활용** (실행).

**더 이상 묻지 마세요:** "Superpowers인가 gstack인가?" **대신 물어보세요:** *의사결정, 컨텍스트, 실행 중 뭔가 빠뜨린 게 없나?*

## 마무리:

Skills는 더 많이 설치한다고 해서 더 강해지는 것이 아닙니다. **실제로 필요한 갭을 채우기 위해 올바른 조각들을 결합하고** 각 레이어가 무엇을 하는지 이해한 후, 자신만의 워크플로우를 조립할 때 더 강해집니다.

---

## 참고자료

- **Superpowers** — [github.com/obra/superpowers](https://github.com/obra/superpowers)
- **gstack** — [github.com/garrytan/gstack](https://github.com/garrytan/gstack)
- **GSD (Get Shit Done)** — [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done) (원본) | [github.com/gsd-build/gsd-2](https://github.com/gsd-build/gsd-2) (v2, 독립형 CLI)

## Google AI의 최근 기사들을 확인해보세요

[당신의 vibe coding을 다음 단계로 끌어올리세요](https://dev.to/googleai/take-your-vibe-coding-to-the-next-level-1ea?bb=262991)

[Google AI](https://dev.to/googleai?bb=262991)

[Mar 26](https://dev.to/googleai/take-your-vibe-coding-to-the-next-level-1ea?bb=262991)

## 당신의 바이브 코딩을 다음 단계로 끌어올리세요

2 min read

[Antigravity를 사용한 크로스 리포지토리 개발](https://dev.to/gdg/cross-repository-development-with-antigravity-26be?bb=262991)

for [Google Developer Group](https://dev.to/gdg?bb=262991)

[Apr 2](https://dev.to/gdg/cross-repository-development-with-antigravity-26be?bb=262991)

## Antigravity를 이용한 크로스 리포지토리 개발

3 min read

[Antigravity 워크플로우를 활용한 AI 기반 저장소 보안 검사](https://dev.to/gdg/ai-powered-repository-security-check-with-antigravity-workflow-5hee?bb=262991)

[

스캐너 축소를 통한 토큰 경제

](https://dev.to/gdg/ai-powered-repository-security-check-with-antigravity-workflow-5hee?bb=262991)

for [Google Developer Group](https://dev.to/gdg?bb=262991)

[Apr 6](https://dev.to/gdg/ai-powered-repository-security-check-with-antigravity-workflow-5hee?bb=262991)

## Antigravity 워크플로우를 활용한 AI 기반 저장소 보안 검사

5 min read