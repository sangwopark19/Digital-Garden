---
title: "get-shit-done/docs/USER-GUIDE.md at main · gsd-build/get-shit-done"
source: "https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md"
author:
  - "[[GitHub]]"
published:
created: 2026-03-31
description: "A light-weight and powerful meta-prompting, context engineering and spec-driven development system for Claude Code by TÂCHES. - get-shit-done/docs/USER-GUIDE.md at main · gsd-build/get-shit-done"
tags:
  - "clippings"
---
## GSD 사용자 가이드

워크플로우, 문제 해결 및 구성에 대한 상세 참고 자료입니다. 빠른 시작 설정은 [README](https://github.com/gsd-build/get-shit-done/blob/main/README.md) 를 참조하세요.

---

## 목차

- [워크플로우 다이어그램](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#workflow-diagrams)
- [UI 디자인 계약](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#ui-design-contract)
- [백로그 & 스레드](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#backlog--threads)
- [워크스트림](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#workstreams)
- [보안](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#security)
- [명령어 참조](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#command-reference)
- [구성 참고 자료](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#configuration-reference)
- [사용 예시](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#usage-examples)
- [문제 해결](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#troubleshooting)
- [복구 빠른 참고 자료](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#recovery-quick-reference)

---

## 워크플로우 다이어그램

### 전체 프로젝트 라이프사이클

```
┌──────────────────────────────────────────────────┐
  │                   NEW PROJECT                    │
  │  /gsd:new-project                                │
  │  Questions -> Research -> Requirements -> Roadmap│
  └─────────────────────────┬────────────────────────┘
                            │
             ┌──────────────▼─────────────┐
             │      FOR EACH PHASE:       │
             │                            │
             │  ┌────────────────────┐    │
             │  │ /gsd:discuss-phase │    │  <- Lock in preferences
             │  └──────────┬─────────┘    │
             │             │              │
             │  ┌──────────▼─────────┐    │
             │  │ /gsd:ui-phase      │    │  <- Design contract (frontend)
             │  └──────────┬─────────┘    │
             │             │              │
             │  ┌──────────▼─────────┐    │
             │  │ /gsd:plan-phase    │    │  <- Research + Plan + Verify
             │  └──────────┬─────────┘    │
             │             │              │
             │  ┌──────────▼─────────┐    │
             │  │ /gsd:execute-phase │    │  <- Parallel execution
             │  └──────────┬─────────┘    │
             │             │              │
             │  ┌──────────▼─────────┐    │
             │  │ /gsd:verify-work   │    │  <- Manual UAT
             │  └──────────┬─────────┘    │
             │             │              │
             │  ┌──────────▼─────────┐    │
             │  │ /gsd:ship          │    │  <- Create PR (optional)
             │  └──────────┬─────────┘    │
             │             │              │
             │     Next Phase?────────────┘
             │             │ No
             └─────────────┼──────────────┘
                            │
            ┌───────────────▼──────────────┐
            │  /gsd:audit-milestone        │
            │  /gsd:complete-milestone     │
            └───────────────┬──────────────┘
                            │
                   Another milestone?
                       │          │
                      Yes         No -> Done!
                       │
               ┌───────▼──────────────┐
               │  /gsd:new-milestone  │
               └──────────────────────┘
```

### 계획 에이전트 조정

```
/gsd:plan-phase N
         │
         ├── Phase Researcher (x4 parallel)
         │     ├── Stack researcher
         │     ├── Features researcher
         │     ├── Architecture researcher
         │     └── Pitfalls researcher
         │           │
         │     ┌──────▼──────┐
         │     │ RESEARCH.md │
         │     └──────┬──────┘
         │            │
         │     ┌──────▼──────┐
         │     │   Planner   │  <- Reads PROJECT.md, REQUIREMENTS.md,
         │     │             │     CONTEXT.md, RESEARCH.md
         │     └──────┬──────┘
         │            │
         │     ┌──────▼───────────┐     ┌────────┐
         │     │   Plan Checker   │────>│ PASS?  │
         │     └──────────────────┘     └───┬────┘
         │                                  │
         │                             Yes  │  No
         │                              │   │   │
         │                              │   └───┘  (loop, up to 3x)
         │                              │
         │                        ┌─────▼──────┐
         │                        │ PLAN files │
         │                        └────────────┘
         └── Done
```

### 검증 아키텍처 (Nyquist 레이어)

plan-phase 연구 중에 GSD는 코드가 작성되기 전에 각 단계 요구사항에 자동화된 테스트 커버리지를 매핑합니다. 이는 Claude의 executor가 작업을 커밋할 때 이미 피드백 메커니즘이 존재하여 몇 초 내에 검증할 수 있도록 보장합니다.

researcher는 기존 테스트 인프라를 감지하고, 각 요구사항을 특정 테스트 명령어에 매핑하며, 구현이 시작되기 전에 생성되어야 하는 테스트 스캐폴딩을 식별합니다 (Wave 0 작업).

plan-checker는 이를 8번째 검증 차원으로 강제합니다: 자동화된 verify 명령어가 없는 작업이 있는 계획은 승인되지 않습니다.

**출력:** `{phase}-VALIDATION.md` -- 해당 단계의 피드백 계약입니다.

**비활성화:** 테스트 인프라가 초점이 아닌 빠른 프로토타이핑 단계에서는 `/gsd:settings` 에서 `workflow.nyquist_validation: false` 를 설정하세요.

### 소급 검증 (/gsd:validate-phase)

Nyquist validation이 존재하기 전에 실행된 단계이거나 기존 테스트 스위트만 있는 기존 코드베이스의 경우, 소급하여 감사하고 커버리지 격차를 채웁니다:

```
/gsd:validate-phase N
         |
         +-- Detect state (VALIDATION.md exists? SUMMARY.md exists?)
         |
         +-- Discover: scan implementation, map requirements to tests
         |
         +-- Analyze gaps: which requirements lack automated verification?
         |
         +-- Present gap plan for approval
         |
         +-- Spawn auditor: generate tests, run, debug (max 3 attempts)
         |
         +-- Update VALIDATION.md
               |
               +-- COMPLIANT -> all requirements have automated checks
               +-- PARTIAL -> some gaps escalated to manual-only
```

감사자는 구현 코드를 절대 수정하지 않으며, 테스트 파일과 VALIDATION.md만 수정합니다. 테스트에서 구현 버그가 발견되면 이를 에스컬레이션으로 플래그하여 사용자가 해결하도록 합니다.

**사용 시기:** Nyquist가 활성화되기 전에 계획된 단계를 실행한 후, 또는 `/gsd:audit-milestone` 이 Nyquist 준수 격차를 드러낸 후.

### 가정 토론 모드

기본적으로 `/gsd:discuss-phase` 는 구현 선호도에 대해 개방형 질문을 합니다. 가정 모드는 이를 반전시킵니다: GSD가 먼저 코드베이스를 읽고, 단계를 구축하는 방법에 대한 구조화된 가정을 표시한 후, 수정 사항만 요청합니다.

**활성화:** `/gsd:settings` 를 통해 `workflow.discuss_mode` 를 `'assumptions'` 로 설정합니다.

**작동 방식:**

1. PROJECT.md, 코드베이스 매핑 및 기존 규칙을 읽습니다
2. 가정 사항의 구조화된 목록을 생성합니다 (기술 선택, 패턴, 파일 위치)
3. 확인, 수정 또는 확장할 수 있도록 가정 사항을 제시합니다
4. CONTEXT.md를 확인된 가정에서 작성합니다

**사용 시기:**

- 이미 자신의 코드베이스를 잘 알고 있는 경험 많은 개발자
- 개방형 질문이 속도를 늦추는 빠른 반복 작업
- 패턴이 잘 정립되어 있고 예측 가능한 프로젝트

[docs/workflow-discuss-mode.md](https://github.com/gsd-build/get-shit-done/blob/main/docs/workflow-discuss-mode.md) 에서 전체 discuss-mode 참조를 확인하세요.

---

## UI 디자인 계약

### 이유

AI가 생성한 프론트엔드는 Claude Code가 UI를 잘못 만들어서가 아니라 실행 전에 디자인 계약이 존재하지 않았기 때문에 시각적으로 일관성이 없습니다. 공유된 간격 척도, 색상 계약 또는 카피라이팅 표준 없이 구축된 5개의 컴포넌트는 5가지 약간 다른 시각적 결정을 만듭니다.

`/gsd:ui-phase` 는 계획 전에 디자인 계약을 잠금합니다. `/gsd:ui-review` 는 실행 후 결과를 감사합니다.

### 명령어

| 명령 | 설명 |
| --- | --- |
| `/gsd:ui-phase [N]` | 프론트엔드 단계를 위한 UI-SPEC.md 디자인 계약 생성 |
| `/gsd:ui-review [N]` | 구현된 UI의 소급 6기둥 시각 감사 |

### 워크플로우: /gsd:ui-phase

**실행 시기:** `/gsd:discuss-phase` 이후, `/gsd:plan-phase` 이전 — 프론트엔드/UI 작업이 있는 페이즈의 경우.

**흐름:**

1. CONTEXT.md, RESEARCH.md, REQUIREMENTS.md를 읽어 기존 결정사항 확인
2. 디자인 시스템 상태 감지 (shadcn components.json, Tailwind config, 기존 토큰)
3. shadcn 초기화 게이트 — React/Next.js/Vite 프로젝트에 없으면 초기화 제안
4. 답변되지 않은 디자인 계약 질문만 요청 (간격, 타이포그래피, 색상, 카피라이팅, 레지스트리 안전성)
5. `{phase}-UI-SPEC.md` 를 페이즈 디렉토리에 작성
6. 6가지 차원(카피라이팅, 비주얼, 색상, 타이포그래피, 간격, 레지스트리 안전성)에 대해 검증
7. BLOCKED인 경우 수정 루프(최대 2회 반복)

**출력:** `.planning/phases/{phase-dir}/` 에 `{padded_phase}-UI-SPEC.md`

### 워크플로우: /gsd:ui-review

**실행 시기:** `/gsd:execute-phase` 또는 `/gsd:verify-work` 이후 — 프론트엔드 코드가 있는 모든 프로젝트에서 실행합니다.

**독립 실행:** GSD 관리 프로젝트뿐만 아니라 모든 프로젝트에서 작동합니다. UI-SPEC.md가 없으면 추상적인 6가지 기둥 표준에 대해 감사합니다.

**6 Pillars (각각 1-4점으로 평가):**

1. 카피라이팅 — CTA 레이블, 빈 상태, 오류 상태
2. 시각 요소 — 초점, 시각적 계층 구조, 아이콘 접근성
3. 색상 — 악센트 사용 규칙, 60/30/10 준수
4. 타이포그래피 — 글꼴 크기/굵기 제약 준수
5. 간격 — 그리드 정렬, 토큰 일관성
6. 경험 디자인 — 로딩/에러/빈 상태 커버리지

**출력: 점수와 상위 3개 우선순위 수정 사항이 포함된** `{padded_phase}-UI-REVIEW.md` 를 phase 디렉토리에 생성합니다.

### 구성

| 설정 | 기본값 | 설명 |
| --- | --- | --- |
| `workflow.ui_phase` | `true` | 프론트엔드 단계를 위한 UI 디자인 계약 생성 |
| `workflow.ui_safety_gate` | `true` | 프론트엔드 단계를 위해 /gsd:ui-phase를 실행하도록 plan-phase 프롬프트 |

둘 다 absent=enabled 패턴을 따릅니다. `/gsd:settings` 를 통해 비활성화할 수 있습니다.

### shadcn 초기화

React/Next.js/Vite 프로젝트의 경우, UI 연구원은 `components.json` 이 없으면 shadcn을 초기화할 것을 제안합니다. 플로우는 다음과 같습니다:

1. `ui.shadcn.com/create` 를 방문하여 프리셋을 구성합니다
2. 사전 설정 문자열 복사
3. `npx shadcn init --preset {paste}`
4. 프리셋은 전체 디자인 시스템을 인코딩합니다 — 색상, 테두리 반경, 폰트

프리셋 문자열은 1급 GSD 계획 아티팩트가 되어 단계와 마일스톤 전반에 걸쳐 재현 가능합니다.

### 레지스트리 안전 게이트

타사 shadcn 레지스트리는 임의의 코드를 주입할 수 있습니다. 안전 게이트에는 다음이 필요합니다:

- `npx shadcn view {component}` — 설치 전에 검사합니다
- `npx shadcn diff {component}` — 공식 버전과 비교합니다

`workflow.ui_safety_gate` 설정 토글로 제어됩니다.

### 스크린샷 저장소

`/gsd:ui-review` 는 Playwright CLI를 통해 스크린샷을 캡처하여 `.planning/ui-reviews/` 에 저장합니다. `.gitignore` 는 바이너리 파일이 git에 도달하는 것을 방지하기 위해 자동으로 생성됩니다. 스크린샷은 `/gsd:complete-milestone` 중에 정리됩니다.

---

## 백로그 & 스레드

### 백로그 주차장

아직 적극적인 계획 준비가 되지 않은 아이디어는 999.x 번호 지정을 사용하여 백로그에 저장되며, 활성 단계 시퀀스 외부에 유지됩니다.

```
/gsd:add-backlog "GraphQL API layer"     # Creates 999.1-graphql-api-layer/
/gsd:add-backlog "Mobile responsive"     # Creates 999.2-mobile-responsive/
```

백로그 항목은 전체 단계 디렉토리를 가지므로, `/gsd:discuss-phase 999.1` 을 사용하여 아이디어를 더 탐색하거나 준비가 되었을 때 `/gsd:plan-phase 999.1` 을 사용할 수 있습니다.

**검토 및 승격** - `/gsd:review-backlog` 를 사용하면 모든 백로그 항목을 표시하고 승격(활성 시퀀스로 이동), 유지(백로그에 남김) 또는 제거(삭제)할 수 있습니다.

### 시드

씨드(Seeds)는 트리거 조건이 있는 미래 지향적 아이디어입니다. 백로그 항목과 달리 씨드는 적절한 마일스톤이 도래하면 자동으로 표시됩니다.

```
/gsd:plant-seed "Add real-time collab when WebSocket infra is in place"
```

시드는 전체 WHY와 WHEN을 보존하여 표시합니다. `/gsd:new-milestone` 은 모든 시드를 스캔하고 일치하는 항목을 제시합니다.

**저장소:** `.planning/seeds/SEED-NNN-slug.md`

### 지속적 컨텍스트 스레드

스레드는 여러 세션에 걸쳐 진행되지만 특정 단계에 속하지 않는 작업을 위한 경량 크로스 세션 지식 저장소입니다.

```
/gsd:thread                              # List all threads
/gsd:thread fix-deploy-key-auth          # Resume existing thread
/gsd:thread "Investigate TCP timeout"    # Create new thread
```

스레드는 `/gsd:pause-work` 보다 가볍습니다 — 단계 상태 없음, 계획 컨텍스트 없음. 각 스레드 파일에는 Goal, Context, References, Next Steps 섹션이 포함됩니다.

스레드는 성숙해지면 페이즈(`/gsd:add-phase`)나 백로그 항목(`/gsd:add-backlog`)으로 승격될 수 있습니다.

**저장소:** `.planning/threads/{slug}.md`

---

## 워크스트림

워크스트림을 사용하면 상태 충돌 없이 여러 마일스톤 영역에서 동시에 작업할 수 있습니다. 각 워크스트림은 자체 격리된 `.planning/` 상태를 가지므로, 워크스트림 간 전환 시 진행 상황이 손상되지 않습니다.

**사용 시기:** 서로 다른 관심 영역(예: 백엔드 API 및 프론트엔드 대시보드)에 걸친 마일스톤 기능을 작업 중이며, 컨텍스트 누수 없이 독립적으로 계획, 실행 또는 논의하고 싶을 때입니다.

### 명령어

| 명령 | 목적 |
| --- | --- |
| `/gsd:workstreams create <name>` | 격리된 계획 상태로 새 워크스트림 생성 |
| `/gsd:workstreams switch <name>` | 활성 컨텍스트를 다른 워크스트림으로 전환 |
| `/gsd:workstreams list` | 모든 워크스트림을 표시하고 어느 것이 활성 상태인지 표시 |
| `/gsd:workstreams complete <name>` | 워크스트림을 완료로 표시하고 상태를 보관하기 |

### 작동 방식

각 작업 스트림은 자체 `.planning/` 디렉토리 서브트리를 유지합니다. 작업 스트림을 전환하면 GSD는 활성 계획 컨텍스트를 교체하여 `/gsd:progress`, `/gsd:discuss-phase`, `/gsd:plan-phase` 및 기타 명령어가 해당 작업 스트림의 상태에서 작동하도록 합니다.

이는 `/gsd:new-workspace` (별도의 저장소 워크트리를 생성)보다 가볍습니다. 작업 스트림은 동일한 코드베이스와 git 히스토리를 공유하지만 계획 아티팩트를 격리합니다.

---

## 보안

### Defense-in-Depth (v1.27)

GSD는 LLM 시스템 프롬프트가 되는 마크다운 파일을 생성합니다. 이는 계획 아티팩트로 흐르는 모든 사용자 제어 텍스트가 잠재적인 간접 프롬프트 인젝션 벡터라는 의미입니다. v1.27은 중앙화된 보안 강화를 도입했습니다:

**경로 순회 방지:** 사용자가 제공한 모든 파일 경로(`--text-file`, `--prd`)는 프로젝트 디렉토리 내에서 확인되도록 검증됩니다. macOS `/var` → `/private/var` 심볼릭 링크 해석이 처리됩니다.

**프롬프트 인젝션 탐지:** `security.cjs` 모듈은 사용자가 제공한 텍스트가 계획 아티팩트에 들어가기 전에 알려진 인젝션 패턴(역할 오버라이드, 명령어 우회, 시스템 태그 인젝션)을 스캔합니다.

**런타임 훅:**

- `gsd-prompt-guard.js` — `.planning/` 에 대한 Write/Edit 호출을 스캔하여 주입 패턴 감지 (항상 활성화, 권고 전용)
- `gsd-workflow-guard.js` — GSD 워크플로우 컨텍스트 외부의 파일 편집에 대해 경고합니다 (`hooks.workflow_guard` 를 통해 선택 가능)

**CI Scanner:** `gsd-ci-scanner.js` 는 모든 에이전트, 워크플로우 및 명령어 파일에서 임베드된 인젝션 벡터를 스캔합니다. 테스트 스위트의 일부로 실행됩니다.

---

### 실행 웨이브 조정

```
/gsd:execute-phase N
         │
         ├── Analyze plan dependencies
         │
         ├── Wave 1 (independent plans):
         │     ├── Executor A (fresh 200K context) -> commit
         │     └── Executor B (fresh 200K context) -> commit
         │
         ├── Wave 2 (depends on Wave 1):
         │     └── Executor C (fresh 200K context) -> commit
         │
         └── Verifier
               └── Check codebase against phase goals
                     │
                     ├── PASS -> VERIFICATION.md (success)
                     └── FAIL -> Issues logged for /gsd:verify-work
```

### 기존 코드베이스 워크플로우 (Brownfield Workflow)

```
/gsd:map-codebase
         │
         ├── Stack Mapper     -> codebase/STACK.md
         ├── Arch Mapper      -> codebase/ARCHITECTURE.md
         ├── Convention Mapper -> codebase/CONVENTIONS.md
         └── Concern Mapper   -> codebase/CONCERNS.md
                │
        ┌───────▼──────────┐
        │ /gsd:new-project │  <- Questions focus on what you're ADDING
        └──────────────────┘
```

---

## 명령어 참조

### 핵심 워크플로우

| 명령 | 목적 | 사용 시기 |
| --- | --- | --- |
| `/gsd:new-project` | 전체 프로젝트 초기화: 질문, 연구, 요구사항, 로드맵 | 새로운 프로젝트 시작 |
| `/gsd:new-project --auto @idea.md` | 문서에서 자동 초기화 | PRD 또는 아이디어 문서 준비 완료 |
| `/gsd:discuss-phase [N]` | 구현 결정사항 캡처 | 계획 전에 빌드 방식을 정하기 위해 |
| `/gsd:ui-phase [N]` | UI 디자인 계약 생성 | 토론 단계 후, 계획 단계 전 (프론트엔드 단계) |
| `/gsd:plan-phase [N]` | 연구 + 계획 + 검증 | 단계를 실행하기 전에 |
| `/gsd:execute-phase <N>` | 모든 계획을 병렬 웨이브로 실행 | 계획이 완료된 후 |
| `/gsd:verify-work [N]` | 자동 진단을 통한 수동 UAT | 실행이 완료된 후 |
| `/gsd:ship [N]` | 검증된 작업에서 PR 생성 | 검증 통과 후 |
| `/gsd:fast <text>` | 인라인 사소한 작업 — 계획 단계를 완전히 건너뜀 | 오타 수정, 설정 변경, 소규모 리팩토링 |
| `/gsd:next` | 상태 자동 감지 및 다음 단계 실행 | 언제든지 — "다음에 뭘 해야 하지?" |
| `/gsd:ui-review [N]` | 소급 6기둥 시각 감사 | 실행 후 또는 verify-work 확인(프론트엔드 프로젝트) |
| `/gsd:audit-milestone` | 마일스톤이 완료 정의를 충족했는지 확인 | 마일스톤 완료 전 |
| `/gsd:complete-milestone` | 아카이브 마일스톤, 태그 릴리스 | 모든 단계 검증됨 |
| `/gsd:new-milestone [name]` | 다음 버전 사이클 시작 | 마일스톤 완료 후 |

### 네비게이션

| 명령 | 목적 | 사용 시기 |
| --- | --- | --- |
| `/gsd:progress` | 상태 표시 및 다음 단계 표시 | 언제든지 -- "내가 어디에 있는가?" |
| `/gsd:resume-work` | 마지막 세션에서 전체 컨텍스트 복원 | 새 세션 시작 |
| `/gsd:pause-work` | 구조화된 인수인계 저장 (HANDOFF.json + continue-here.md) | 단계 중간에 중단 |
| `/gsd:session-report` | 작업 및 결과를 포함한 세션 요약 생성 | 세션 종료, 이해관계자 공유 |
| `/gsd:help` | 모든 명령어 표시 | 빠른 참조 |
| `/gsd:update` | GSD를 변경 로그 미리보기로 업데이트 | 새 버전 확인 |
| `/gsd:join-discord` | Discord 커뮤니티 초대 열기 | 질문 또는 커뮤니티 |

### 단계 관리

| 명령 | 목적 | 사용 시기 |
| --- | --- | --- |
| `/gsd:add-phase` | 로드맵에 새로운 단계 추가 | 초기 계획 후 범위 확대 |
| `/gsd:insert-phase [N]` | 긴급 작업 삽입 (십진법 번호 매기기) | 마일스톤 중간 긴급 수정 |
| `/gsd:remove-phase [N]` | 향후 단계 제거 및 번호 재지정 | 기능 범위 축소 |
| `/gsd:list-phase-assumptions [N]` | Claude의 의도된 접근 방식 미리보기 | 계획 전에 방향 검증 |
| `/gsd:plan-milestone-gaps` | 감사 격차에 대한 단계 생성 | 감사에서 누락된 항목 발견 후 |
| `/gsd:research-phase [N]` | 심층 생태계 조사만 수행 | 복잡하거나 낯선 도메인 |

### 기존 코드베이스 및 유틸리티

| 명령                           | 목적                     | 사용 시기                              |
| ---------------------------- | ---------------------- | ---------------------------------- |
| `/gsd:map-codebase`          | 기존 코드베이스 분석            | `/gsd:new-project` 실행 전 기존 코드      |
| `/gsd:quick`                 | GSD를 통한 임시 작업 보장       | 버그 수정, 소규모 기능, 설정 변경               |
| `/gsd:debug [desc]`          | 지속적인 상태를 유지하는 체계적인 디버깅 | 문제가 발생했을 때                         |
| `/gsd:forensics`             | 워크플로우 실패에 대한 진단 보고서    | 상태, 아티팩트 또는 git 히스토리가 손상된 것으로 보일 때 |
| `/gsd:add-todo [desc]`       | 나중을 위해 아이디어 캡처하기       | 세션 중에 뭔가 생각하기                      |
| `/gsd:check-todos`           | 대기 중인 할 일 목록 표시        | 캡처된 아이디어 검토하기                      |
| `/gsd:settings`              | 워크플로우 토글 및 모델 프로필 구성하기 | 모델 변경, 에이전트 토글                     |
| `/gsd:set-profile <profile>` | 빠른 프로필 전환              | 비용/품질 트레이드오프 변경                    |
| `/gsd:reapply-patches`       | 업데이트 후 로컬 수정사항 복원      | 로컬 편집이 있었다면 `/gsd:update` 실행 후     |

### 코드 품질 및 검토

| 명령 | 목적 | 사용 시기 |
| --- | --- | --- |
| `/gsd:review --phase N` | AI 간 교차 피어 리뷰(외부 CLI에서) | 실행 전에 계획을 검증하기 위해 |
| `/gsd:pr-branch` | `.planning/` 커밋을 필터링하여 PR 브랜치 정리 | 계획 없는 diff로 PR을 생성하기 전에 |
| `/gsd:audit-uat` | 모든 단계에서 감사 검증 부채 확인 | 마일스톤 완료 전 |

### 백로그 & 스레드

| 명령 | 목적 | 사용 시기 |
| --- | --- | --- |
| `/gsd:add-backlog <desc>` | 백로그 주차장에 아이디어 추가 (999.x) | 활성 계획 준비가 안 된 아이디어 |
| `/gsd:review-backlog` | 백로그 항목 승격/유지/제거 | 새로운 마일스톤 전에 우선순위 지정 |
| `/gsd:plant-seed <idea>` | 트리거 조건이 있는 미래 지향적 아이디어 | 향후 마일스톤에서 나타나야 할 아이디어 |
| `/gsd:thread [name]` | 지속적인 컨텍스트 스레드 | 단계 구조 외부의 크로스 세션 작업 |

---

## 구성 참조

GSD는 프로젝트 설정을 `.planning/config.json` 에 저장합니다. `/gsd:new-project` 중에 구성하거나 나중에 `/gsd:settings` 로 업데이트할 수 있습니다.

### 전체 config.json 스키마

```
{
  "mode": "interactive",
  "granularity": "standard",
  "model_profile": "balanced",
  "planning": {
    "commit_docs": true,
    "search_gitignored": false
  },
  "workflow": {
    "research": true,
    "plan_check": true,
    "verifier": true,
    "nyquist_validation": true,
    "ui_phase": true,
    "ui_safety_gate": true,
    "research_before_questions": false,
    "discuss_mode": "standard",
    "skip_discuss": false
  },
  "resolve_model_ids": "anthropic",
  "hooks": {
    "context_warnings": true,
    "workflow_guard": false
  },
  "git": {
    "branching_strategy": "none",
    "phase_branch_template": "gsd/phase-{phase}-{slug}",
    "milestone_branch_template": "gsd/{milestone}-{slug}",
    "quick_branch_template": null
  }
}
```

### 핵심 설정

| 설정 | 옵션 | 기본값 | 제어하는 항목 |
| --- | --- | --- | --- |
| `mode` | `interactive`, `yolo` | `interactive` | `yolo` 는 결정을 자동으로 승인하고, `interactive` 는 각 단계에서 확인합니다. |
| `granularity` | `coarse`, `standard`, `fine` | `standard` | 단계 세분화: 범위를 얼마나 세밀하게 나누는지 (3-5, 5-8, 또는 8-12 단계) |
| `model_profile` | `quality`, `balanced`, `budget`, `inherit` | `balanced` | 각 에이전트의 모델 계층 (아래 표 참조) |

### 계획 설정

| 설정 | 옵션 | 기본값 | 제어하는 항목 |
| --- | --- | --- | --- |
| `planning.commit_docs` | `true`, `false` | `true` | `.planning/` 파일이 git에 커밋되는지 여부 |
| `planning.search_gitignored` | `true`, `false` | `false` | 광범위한 검색에 `--no-ignore` 를 추가하여 `.planning/을 포함시킵니다` |

> **참고:** `.planning/` 이 `.gitignore` 에 있으면, 설정 값과 관계없이 `commit_docs` 는 자동으로 `false` 입니다.

### 워크플로우 토글

| 설정 | 옵션 | 기본값 | 제어하는 항목 |
| --- | --- | --- | --- |
| `workflow.research` | `true`, `false` | `true` | 계획 수립 전 도메인 조사 |
| `workflow.plan_check` | `true`, `false` | `true` | 계획 검증 루프 (최대 3회 반복) |
| `workflow.verifier` | `true`, `false` | `true` | 단계 목표에 대한 실행 후 검증 |
| `workflow.nyquist_validation` | `true`, `false` | `true` | 계획 단계 중 검증 아키텍처 연구; 8번째 계획 검사 차원 |
| `workflow.ui_phase` | `true`, `false` | `true` | 프론트엔드 단계를 위한 UI 디자인 계약 생성 |
| `workflow.ui_safety_gate` | `true`, `false` | `true` | 프론트엔드 단계를 위해 /gsd:ui-phase를 실행하도록 plan-phase 프롬프트 |
| `workflow.research_before_questions` | `true`, `false` | `false` | 토론 후가 아닌 토론 전에 연구 실행 |
| `workflow.discuss_mode` | `standard`, `assumptions` | `standard` | 토론 스타일: 개방형 질문 vs. 코드베이스 기반 가정 |
| `workflow.skip_discuss` | `true`, `false` | `false` | 자율 모드에서 토론 단계 완전히 건너뛰기; ROADMAP 단계 목표에서 최소한의 CONTEXT.md 작성 |

### Hook 설정

| 설정 | 옵션 | 기본값 | 제어하는 항목 |
| --- | --- | --- | --- |
| `hooks.context_warnings` | `true`, `false` | `true` | 컨텍스트 윈도우 사용량 경고 |
| `hooks.workflow_guard` | `true`, `false` | `false` | GSD 워크플로우 컨텍스트 외부의 파일 편집에 대해 경고 |

익숙한 도메인에서 또는 토큰을 절약할 때 워크플로우 토글을 비활성화하여 단계 속도 향상

### Git 브랜칭

| 설정 | 옵션 | 기본값 | 제어하는 항목 |
| --- | --- | --- | --- |
| `git.branching_strategy` | `없음`, `phase`, `phase` | `none` | 브랜치가 생성되는 시기 및 방법 |
| `git.phase_branch_template` | 템플릿 문자열 | `gsd/phase-{phase}-{slug}` | 페이즈 전략을 위한 브랜치 이름 |
| `git.milestone_branch_template` | 템플릿 문자열 | `gsd/{milestone}-{slug}` | 마일스톤 전략을 위한 브랜치 이름 |
| `git.quick_branch_template` | 템플릿 문자열 또는 `null` | `null` | `/gsd:quick` 작업을 위한 선택적 브랜치 이름 |

**브랜칭 전략 설명:**

| 전략 | 브랜치 생성 | 범위 | 최적 사용 사례 |
| --- | --- | --- | --- |
| `none` | 절대 안 함 | 해당 없음 | 솔로 개발, 간단한 프로젝트 |
| `phase` | 각 `execute-phase에서` | 브랜치당 하나의 페이즈 | 단계별 코드 검토, 세분화된 롤백 |
| `milestone` | 첫 번째 `execute-phase에서` | 모든 페이즈가 하나의 브랜치를 공유 | 릴리스 브랜치, 버전당 PR |

**템플릿 변수:** `{phase}` = 0으로 채워진 숫자 (예: "03"), `{slug}` = 소문자 하이픈 이름, `{milestone}` = 버전 (예: "v1.0"), `{num}` / `{quick}` = 빠른 작업 ID (예: "260317-abc").

빠른 작업 브랜칭 예시:

```
"git": {
  "quick_branch_template": "gsd/quick-{num}-{slug}"
}
```

### 모델 프로필 (에이전트별 분석)

| 에이전트 | `quality` | `balanced` | `budget` | `inherit` |
| --- | --- | --- | --- | --- |
| gsd-planner | Opus | Opus | Sonnet | 상속 |
| gsd-roadmapper | Opus | Sonnet | Sonnet | 상속 |
| gsd-executor | Opus | Sonnet | Sonnet | 상속 |
| gsd-phase-researcher | Opus | Sonnet | Haiku | 상속 |
| gsd-project-researcher | Opus | Sonnet | Haiku | 상속 |
| gsd-research-synthesizer | Sonnet | Sonnet | Haiku | 상속 |
| gsd-debugger | Opus | Sonnet | Sonnet | 상속 |
| gsd-codebase-mapper | Sonnet | Haiku | Haiku | 상속 |
| gsd-verifier | Sonnet | Sonnet | Haiku | 상속 |
| gsd-plan-checker | Sonnet | Sonnet | Haiku | 상속 |
| gsd-integration-checker | Sonnet | Sonnet | Haiku | 상속 |

**프로필 철학:**

- **quality** -- 모든 의사결정 에이전트에는 Opus를, 읽기 전용 검증에는 Sonnet을 사용합니다. 할당량이 있고 작업이 중요할 때 사용하세요.
- **balanced** -- 계획 수립(아키텍처 결정이 이루어지는 곳)에만 Opus를, 나머지 모든 작업에는 Sonnet을 사용합니다. 타당한 이유로 기본값입니다.
- **budget** -- 코드를 작성하는 모든 작업에는 Sonnet을, 연구 및 검증에는 Haiku를 사용합니다. 대량 작업이나 덜 중요한 단계에서 사용하세요.
- **inherit** -- 모든 에이전트가 현재 세션 모델을 사용합니다. 모델을 동적으로 전환할 때(예: OpenCode `/model`) 또는 예상치 못한 API 비용을 피하기 위해 Claude Code를 비-Anthropic 제공자(OpenRouter, 로컬 모델)와 함께 사용할 때 최적입니다. 비-Claude 런타임(Codex, OpenCode, Gemini CLI)의 경우 설치 프로그램이 자동으로 `resolve_model_ids: "omit"` 을 설정합니다 -- [비-Claude 런타임](https://github.com/gsd-build/get-shit-done/blob/main/docs/USER-GUIDE.md#using-non-claude-runtimes-codex-opencode-gemini-cli) 을 참조하세요.

---

## 사용 예시

### 새 프로젝트 (전체 사이클)

```
claude --dangerously-skip-permissions
/gsd:new-project            # Answer questions, configure, approve roadmap
/clear
/gsd:discuss-phase 1        # Lock in your preferences
/gsd:ui-phase 1             # Design contract (frontend phases)
/gsd:plan-phase 1           # Research + plan + verify
/gsd:execute-phase 1        # Parallel execution
/gsd:verify-work 1          # Manual UAT
/gsd:ship 1                 # Create PR from verified work
/gsd:ui-review 1            # Visual audit (frontend phases)
/clear
/gsd:next                   # Auto-detect and run next step
...
/gsd:audit-milestone        # Check everything shipped
/gsd:complete-milestone     # Archive, tag, done
/gsd:session-report         # Generate session summary
```

### 기존 문서에서 새 프로젝트 만들기

```
/gsd:new-project --auto @prd.md   # Auto-runs research/requirements/roadmap from your doc
/clear
/gsd:discuss-phase 1               # Normal flow from here
```

### 기존 코드베이스

```
/gsd:map-codebase           # Analyze what exists (parallel agents)
/gsd:new-project            # Questions focus on what you're ADDING
# (normal phase workflow from here)
```

### 빠른 버그 수정

```
/gsd:quick
> "Fix the login button not responding on mobile Safari"
```

### 휴식 후 재개

```
/gsd:progress               # See where you left off and what's next
# or
/gsd:resume-work            # Full context restoration from last session
```

### 릴리스 준비

```
/gsd:audit-milestone        # Check requirements coverage, detect stubs
/gsd:plan-milestone-gaps    # If audit found gaps, create phases to close them
/gsd:complete-milestone     # Archive, tag, done
```

### 속도 대 품질 사전 설정

| 시나리오 | 모드 | 세분성 | 프로필 | 연구 | 계획 확인 | 검증자 |
| --- | --- | --- | --- | --- | --- | --- |
| 프로토타이핑 | `yolo` | `coarse` | `budget` | off | off | off |
| 일반 개발 | `interactive` | `standard` | `balanced` | on | on | on |
| 프로덕션 | `interactive` | `fine` | `quality` | on | on | on |

**자율 모드에서 discuss-phase 건너뛰기:** PROJECT.md에 이미 확립된 선호도가 캡처되어 있는 상태에서 `yolo` 모드로 실행할 때, `/gsd:settings` 를 통해 `workflow.skip_discuss: true` 를 설정하세요. 이렇게 하면 discuss-phase를 완전히 건너뛰고 ROADMAP phase 목표에서 파생된 최소한의 CONTEXT.md를 작성합니다. PROJECT.md와 규칙이 충분히 포괄적이어서 논의가 새로운 정보를 추가하지 않을 때 유용합니다.

### 마일스톤 중간 범위 변경

```
/gsd:add-phase              # Append a new phase to the roadmap
# or
/gsd:insert-phase 3         # Insert urgent work between phases 3 and 4
# or
/gsd:remove-phase 7         # Descope phase 7 and renumber
```

### 멀티 프로젝트 워크스페이스

격리된 GSD 상태로 여러 저장소 또는 기능을 병렬로 작업합니다.

```
# Create a workspace with repos from your monorepo
/gsd:new-workspace --name feature-b --repos hr-ui,ZeymoAPI

# Feature branch isolation — worktree of current repo with its own .planning/
/gsd:new-workspace --name feature-b --repos .

# Then cd into the workspace and initialize GSD
cd ~/gsd-workspaces/feature-b
/gsd:new-project

# List and manage workspaces
/gsd:list-workspaces
/gsd:remove-workspace feature-b
```

각 워크스페이스는 다음을 제공합니다:

- 자체 `.planning/` 디렉토리 (소스 저장소와 완전히 독립적)
- Git 워크트리(기본값) 또는 지정된 저장소의 클론
- 멤버 저장소를 추적하는 `WORKSPACE.md` 매니페스트

---

## 문제 해결

### "프로젝트가 이미 초기화되었습니다"

`/gsd:new-project` 를 실행했지만 `.planning/PROJECT.md` 가 이미 존재합니다. 이는 안전 검사입니다. 처음부터 시작하려면 `.planning/` 디렉토리를 먼저 삭제하세요.

### 긴 세션 중 컨텍스트 저하

주요 명령어 사이에 컨텍스트 윈도우를 초기화하세요: Claude Code에서 `/clear` 를 사용합니다. GSD는 깨끗한 컨텍스트를 중심으로 설계되었습니다 -- 모든 서브에이전트는 깨끗한 200K 윈도우를 받습니다. 메인 세션에서 품질이 저하되면 `/gsd:resume-work` 또는 `/gsd:progress` 를 사용하여 상태를 복원하세요.

### 계획이 잘못되었거나 정렬되지 않은 것 같음

`/gsd:discuss-phase [N]` 을 계획하기 전에 실행하세요. 대부분의 계획 품질 문제는 Claude가 `CONTEXT.md` 가 방지했을 가정을 하기 때문에 발생합니다. `/gsd:dry-run` 을 실행하여 계획에 커밋하기 전에 Claude가 무엇을 하려고 하는지 확인할 수도 있습니다.

### 실행 실패 또는 스텁 생성

계획이 너무 야심차지 않았는지 확인하세요. 계획은 최대 2-3개의 작업을 포함해야 합니다. 작업이 너무 크면 단일 컨텍스트 윈도우가 안정적으로 생성할 수 있는 범위를 초과합니다. 더 작은 범위로 다시 계획하세요.

### 현재 위치를 잃어버렸을 때

`/gsd:progress` 를 실행하세요. 모든 상태 파일을 읽고 현재 위치와 다음 단계를 정확히 알려줍니다.

### 실행 후 무언가를 변경해야 할 때

`/gsd:execute-phase` 를 다시 실행하지 마세요. 대상 수정을 위해 `/gsd:quick` 을 사용하거나, UAT를 통해 체계적으로 문제를 식별하고 해결하려면 `/gsd:verify-work` 를 사용하세요.

### 모델 비용이 너무 높음

예산 프로필로 전환하세요: `/gsd:set-profile budget`. 도메인이 익숙하다면(또는 Claude에 익숙하다면) `/gsd:settings` 를 통해 연구 및 계획 검증 에이전트를 비활성화하세요.

### Claude가 아닌 런타임 사용 (Codex, OpenCode, Gemini CLI)

Claude가 아닌 런타임을 위해 GSD를 설치한 경우, 설치 프로그램이 이미 모델 해석을 구성했으므로 모든 에이전트가 런타임의 기본 모델을 사용합니다. 수동 설정이 필요하지 않습니다. 구체적으로, 설치 프로그램은 구성에서 `resolve_model_ids: "omit"` 을 설정하여 GSD가 Anthropic 모델 ID 해석을 건너뛰고 런타임이 자체 기본 모델을 선택하도록 합니다.

Claude가 아닌 런타임에서 다양한 모델을 다양한 에이전트에 할당하려면, 런타임이 인식하는 완전한 형식의 모델 ID를 사용하여 `.planning/config.json` 에 `model_overrides` 를 추가하세요:

```
{
  "resolve_model_ids": "omit",
  "model_overrides": {
    "gsd-planner": "o3",
    "gsd-executor": "o4-mini",
    "gsd-debugger": "o3"
  }
}
```

설치 프로그램은 Gemini CLI, OpenCode, Codex에 대해 `resolve_model_ids: "omit"` 을 자동으로 구성합니다. Claude가 아닌 런타임을 수동으로 설정하는 경우, `.planning/config.json` 에 직접 추가하세요.

[구성 참조](https://github.com/gsd-build/get-shit-done/blob/main/docs/CONFIGURATION.md#non-claude-runtimes-codex-opencode-gemini-cli) 에서 전체 설명을 확인하세요.

### Claude Code를 Non-Anthropic 제공자(OpenRouter, Local)와 함께 사용하기

GSD 서브에이전트가 Anthropic 모델을 호출하고 OpenRouter 또는 로컬 제공자를 통해 비용을 지불하는 경우, `inherit` 프로필로 전환하세요: `/gsd:set-profile inherit`. 이렇게 하면 모든 에이전트가 특정 Anthropic 모델 대신 현재 세션 모델을 사용합니다. `/gsd:settings` → Model Profile → Inherit도 참조하세요.

### 민감한/개인 프로젝트에서 작업하기

`commit_docs: false` 를 `/gsd:new-project` 중에 설정하거나 `/gsd:settings` 를 통해 설정하세요. `.planning/` 을 `.gitignore` 에 추가하세요. 계획 아티팩트는 로컬에만 유지되며 git에 절대 커밋되지 않습니다.

### GSD 업데이트가 내 로컬 변경사항을 덮어썼습니다

v1.17 이후로, 설치 프로그램은 로컬에서 수정된 파일을 `gsd-local-patches/` 에 백업합니다. `/gsd:reapply-patches` 를 실행하여 변경 사항을 다시 병합하세요.

### 워크플로우 진단 (/gsd:forensics)

워크플로우가 명확하지 않은 방식으로 실패할 때 -- 계획이 존재하지 않는 파일을 참조하거나, 실행이 예상치 못한 결과를 생성하거나, 상태가 손상된 것으로 보일 때 -- `/gsd:forensics` 를 실행하여 진단 보고서를 생성하세요.

**확인하는 항목:**

- Git 히스토리 이상 (고아 커밋, 예상치 못한 브랜치 상태, 리베이스 아티팩트)
- 아티팩트 무결성 (누락되거나 형식이 잘못된 계획 파일, 끊어진 상호 참조)
- 상태 불일치 (ROADMAP 상태 vs. 실제 파일 존재 여부, 설정 드리프트)

**출력:** `.planning/forensics/` 에 작성된 진단 보고서로, 발견 사항 및 제안된 개선 단계를 포함합니다.

### 서브에이전트가 실패한 것처럼 보이지만 작업이 완료됨

Claude Code 분류 버그에 대한 알려진 해결 방법이 존재합니다. GSD의 오케스트레이터 (execute-phase, quick)는 실패를 보고하기 전에 실제 출력을 샘플 검사합니다. 실패 메시지가 표시되지만 커밋이 이루어진 경우, `git log` 를 확인하세요 -- 작업이 성공했을 수 있습니다.

### 병렬 실행으로 인한 빌드 잠금 오류

사전 커밋 훅 실패, cargo lock 경합, 또는 병렬 웨이브 실행 중 30분 이상의 실행 시간이 표시되면, 이는 여러 에이전트가 동시에 빌드 도구를 트리거하기 때문입니다. GSD는 v1.26 이후 이를 자동으로 처리합니다 — 병렬 에이전트는 커밋 시 `--no-verify` 를 사용하고 오케스트레이터는 각 웨이브 후에 훅을 한 번 실행합니다. 이전 버전을 사용 중인 경우, 프로젝트의 `CLAUDE.md` 에 다음을 추가하세요:

```
## Git Commit Rules for Agents
All subagent/executor commits MUST use \`--no-verify\`.
```

병렬 실행을 완전히 비활성화하려면: `/gsd:settings` → `parallelization.enabled` 을 `false` 로 설정합니다.

### Windows: 보호된 디렉토리에서 설치 충돌

Windows에서 설치 프로그램이 `EPERM: operation not permitted, scandir` 로 충돌하는 경우, 이는 OS 보호 디렉토리(예: Chromium 브라우저 프로필)로 인해 발생합니다. v1.24 이후로 수정되었으므로 최신 버전으로 업데이트하세요. 임시 해결책으로, 설치 프로그램을 실행하기 전에 문제가 있는 디렉토리의 이름을 일시적으로 변경하세요.

---

## 복구 빠른 참조

| 문제 | 해결책 |
| --- | --- |
| 컨텍스트 손실 / 새 세션 | `/gsd:resume-work` 또는 `/gsd:progress` |
| 페이즈가 잘못되었습니다 | `git revert` 로 페이즈 커밋을 되돌린 후 다시 계획하세요 |
| 범위를 변경해야 합니다 | `/gsd:add-phase`, `/gsd:insert-phase`, 또는 `/gsd:remove-phase` |
| 마일스톤 감사에서 격차를 발견했습니다 | `/gsd:plan-milestone-gaps` |
| 뭔가 깨졌어요 | `/gsd:debug "description"` |
| 워크플로우 상태가 손상된 것으로 보입니다 | `/gsd:forensics` |
| 빠른 표적 수정 | `/gsd:quick` |
| 계획이 당신의 비전과 맞지 않습니다 | `/gsd:discuss-phase [N]` 을 실행한 후 다시 계획하세요 |
| 비용이 높게 실행 중입니다 | `/gsd:set-profile budget` 를 실행하고 `/gsd:settings` 에서 에이전트를 비활성화하세요 |
| 업데이트로 인해 로컬 변경사항 손상됨 | `/gsd:reapply-patches` |
| 이해관계자를 위한 세션 요약을 원합니다 | `/gsd:session-report` |
| 다음 단계가 무엇인지 모르겠습니다 | `/gsd:next` |
| 병렬 실행 빌드 오류 | GSD 업데이트 또는 `parallelization.enabled: false 설정` |

---

## 프로젝트 파일 구조

참고로, GSD가 프로젝트에 생성하는 내용은 다음과 같습니다:

```
.planning/
  PROJECT.md              # Project vision and context (always loaded)
  REQUIREMENTS.md         # Scoped v1/v2 requirements with IDs
  ROADMAP.md              # Phase breakdown with status tracking
  STATE.md                # Decisions, blockers, session memory
  config.json             # Workflow configuration
  MILESTONES.md           # Completed milestone archive
  HANDOFF.json            # Structured session handoff (from /gsd:pause-work)
  research/               # Domain research from /gsd:new-project
  reports/                # Session reports (from /gsd:session-report)
  todos/
    pending/              # Captured ideas awaiting work
    done/                 # Completed todos
  debug/                  # Active debug sessions
    resolved/             # Archived debug sessions
  codebase/               # Brownfield codebase mapping (from /gsd:map-codebase)
  phases/
    XX-phase-name/
      XX-YY-PLAN.md       # Atomic execution plans
      XX-YY-SUMMARY.md    # Execution outcomes and decisions
      CONTEXT.md          # Your implementation preferences
      RESEARCH.md         # Ecosystem research findings
      VERIFICATION.md     # Post-execution verification results
      XX-UI-SPEC.md       # UI design contract (from /gsd:ui-phase)
      XX-UI-REVIEW.md     # Visual audit scores (from /gsd:ui-review)
  ui-reviews/             # Screenshots from /gsd:ui-review (gitignored)
```