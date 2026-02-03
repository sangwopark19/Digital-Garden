# The Longform Guide to Everything Claude Code

**작성자:** cogsec (@affaanmustafa)
**날짜:** 오전 3:19 · 2026년 1월 22일
**원본 링크:** https://x.com/affaanmustafa/status/2014040193557471352

---

## 📊 게시물 통계

- 💬 답글: 24
- 🔄 재게시: 146
- ❤️ 마음에 들어요: 1,100
- 👁️ 조회수: 31만

---

## 📝 소개

In "The Shorthand Guide to Everything Claude Code", I covered the foundational setup: skills and commands, hooks, subagents, MCPs, plugins, and the configuration patterns that form the backbone of an effective Claude Code workflow. Its a setup guide and the base infrastructure.

**이전 게시물 "The Shorthand Guide to Everything Claude Code"에서는** 기초 설정을 다뤘습니다:
- Skills and commands
- Hooks
- Subagents
- MCPs
- Plugins
- 효과적인 Claude Code 워크플로우의 기반이 되는 설정 패턴들

### 이 Longform 가이드의 목적

This longform guide goes the techniques that separate productive sessions from wasteful ones. If you haven't read the Shorthand Guide, go back and set up your configs first.

**What follows assumes you have skills, agents, hooks, and MCPs already configured and working.**

이 가이드는 생산적인 세션과 비효율적인 세션을 구분하는 기술들을 다룹니다.

### 주요 주제

The themes here:
- ✅ Token economics
- ✅ Memory persistence
- ✅ Verification patterns
- ✅ Parallelization strategies
- ✅ Compound effects of building reusable workflows

These are the patterns I've refined over 10+ months of daily use that make the difference between being plagued by context rot within the first hour, versus maintaining productive sessions for hours.

---

## 🧠 Context & Memory Management

### 세션 간 메모리 공유

For sharing memory across sessions, a skill or command that summarizes and checks in on progress then saves to a `.tmp` file in your `.claude` folder and appends to it until the end of your session is the best bet.

**다음 날 진행 방법:**
1. 이전 세션의 임시 파일을 컨텍스트로 사용
2. 새 파일을 각 세션에 대해 생성 (이전 컨텍스트 오염 방지)
3. 결국 큰 폴더의 세션 로그 생성

**세션 파일에 포함되어야 할 내용:**
- 🎯 어떤 접근 방식이 작동했는지 (증거 포함)
- ❌ 시도했지만 작동하지 않은 접근 방식
- ⏳ 시도되지 않은 접근 방식
- 📋 남은 것들

**예시:** https://github.com/affaan-m/everything-claude-code/tree/main/examples/sessions

### 전략적으로 Context 지우기

Once you have your plan set and context cleared (default option in plan mode in claude code now), you can work from the plan.

**Auto-compact vs Strategic Compact:**

- **Auto-compact:** Arbitrary 시점에 발생, 종종 작업 중간에 발생
- **Strategic compact:** 논리적 페이즈를 통해 컨텍스트 보존

```bash
#!/bin/bash
# Strategic Compact Suggester
# Runs on PreToolUse to suggest manual compaction at logical intervals

COUNTER_FILE="/tmp/claude-tool-count-$$"
THRESHOLD=${COMPACT_THRESHOLD:-50}

# Initialize or increment counter
if [ -f "$COUNTER_FILE" ]; then
    count=$(cat "$COUNTER_FILE")
    count=$((count + 1))
    echo "$count" > "$COUNTER_FILE"
else
    echo "1" > "$COUNTER_FILE"
    count=1
fi

# Suggest compact after threshold tool calls
if [ "$count" -eq "$THRESHOLD" ]; then
    echo "[StrategicCompact] $THRESHOLD tool calls reached - consider /compact if transitioning phases" >&2
fi
```

**Hook 설정:** Edit/Write 작업 시 PreToolUse에 연결
→ 컨텍스트 축적 시 조언받음

---

## 🔧 Dynamic System Prompt Injection

One pattern I picked up and am trial running is: instead of solely putting everything in CLAUDE.md (user scope) or `.claude/rules/` (project scope) which loads every session, use CLI flags to inject context dynamically.

```bash
claude --system-prompt "$(cat memory.md)"
```

### 왜 이것이 중요한가?

**@file 참조 vs System Prompt Injection:**

| 방식 | 방법 | 장점 |
|------|------|------|
| @memory.md | Read tool로 파일 읽음 | - |
| System Prompt | 대화 시작 전 주입 | 높은 권한, 빠름, 효율적 |

**명령 우선순위:**
1. System prompt (최고 권한)
2. User messages
3. Tool results

### 실전 설정

```bash
# 일일 개발
alias claude-dev='claude --system-prompt "$(cat ~/.claude/contexts/dev.md)"'

# PR 리뷰 모드
alias claude-review='claude --system-prompt "$(cat ~/.claude/contexts/review.md)"'

# 연구/탐색 모드
alias claude-research='claude --system-prompt "$(cat ~/.claude/contexts/research.md)"'
```

**장점:**
- ✅ CLI 레벨 권한 (도구 호출 없음)
- ✅ 더 빠름
- ✅ Token 효율적

---

## 💾 Advanced: Memory Persistence Hooks

Hooks that help with memory that most people don't know about or don't utilize:

### 세션 라이프사이클

```
SESSION 1                 SESSION 2
─────────                 ─────────
[Start]                   [Start]
  │                         │
  ▼                         ▼
┌──────────────┐          ┌──────────────┐
│ SessionStart │◄─── reads ──── │ SessionStart │◄── loads previous
│ Hook         │ nothing yet    │ Hook         │ context
└──────┬───────┘               └──────┬───────┘
       │                              │
       ▼                              ▼
   [Working]                     [Working]
       │                        (informed)
       ▼                              │
   ┌──────────────┐                  ▼
   │ PreCompact   │──► saves state
   │ Hook         │ before summary
   └──────┬───────┘                  │
          │                          ▼
          ▼                     [Continue...]
     [Compacted]
          │
          ▼
   ┌──────────────┐
   │ Stop Hook    │──► persists to ──────────►
   │ (session-end)│ ~/.claude/sessions/
   └──────────────┘
```

### Hook Types

| Hook | 시점 | 용도 |
|------|------|------|
| **PreCompact** | Context compaction 전 | Important state 파일에 저장 |
| **SessionStart** | 새 세션 시작 | 이전 컨텍스트 자동 로드 |
| **Stop** | 세션 종료 | 학습 내용을 파일에 저장 |

### Memory Persistent Hooks 설정

```json
{
  "hooks": {
    "PreCompact": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/memory-persistence/pre-compact.sh"
      }]
    }],
    "SessionStart": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/memory-persistence/session-start.sh"
      }]
    }],
    "Stop": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/memory-persistence/session-end.sh"
      }]
    }]
  }
}
```

**각 스크립트의 역할:**
- `pre-compact.sh`: Compaction 이벤트 로깅, Active session 파일 타임스탐프 업데이트
- `session-start.sh`: 최근 세션 파일 확인 (지난 7일), 사용 가능한 컨텍스트 알림
- `session-end.sh`: 일일 세션 파일 생성/업데이트, 시작/종료 시간 추적

---

## 📚 Continuous Learning / Memory

### 문제: 반복되는 실수

We talked about continuous memory updating in the form of updating codemaps, but this applies to other things too such as learning from mistakes.

**증상:**
- Same problem으로 반복해서 재prompt
- 같은 답변을 또 들음
- "이미 말했잖아!"라는 좌절감

### 해결책: Automatic Skill Creation

When Claude Code discovers something that isn't trivial, save that knowledge as a new skill. Next time a similar problem comes up, the skill gets loaded automatically.

**Skills를 Stop hook로 자동 생성:**

```bash
#!/bin/bash
# Why Stop hook instead of UserPromptSubmit?
# - UserPromptSubmit: Every message마다 실행 (오버헤드 크음)
# - Stop: Session 종료 시 1회 실행 (가벼움)
# - 완전한 세션 평가 (piecemeal 아님)
```

### 설치

```bash
# Clone to skills folder
git clone https://github.com/affaan-m/everything-claude-code.git ~/.claude/skills/everything-claude-code

# Or just grab the continuous-learning skill
mkdir -p ~/.claude/skills/continuous-learning
curl -sL https://raw.githubusercontent.com/affaan-m/everything-claude-code/main/skills/continuous-learning/evaluate-session.sh > ~/.claude/skills/continuous-learning/evaluate-session.sh
chmod +x ~/.claude/skills/continuous-learning/evaluate-session.sh
```

### Hook 설정

```json
{
  "hooks": {
    "Stop": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/skills/continuous-learning/evaluate-session.sh"
      }]
    }]
  }
}
```

**작동 방식:**
1. Stop hook이 세션 종료 시 활성화
2. 스크립트가 세션 분석
3. 추출할 가치 있는 패턴 찾기:
   - Error resolutions
   - Debugging techniques
   - Workarounds
   - Project-specific patterns
4. Reusable skills로 저장 (`~/.claude/skills/learned/`)

### 수동 추출: /learn 명령어

세션 중간에 바로 패턴을 추출할 수 있습니다:

```bash
/learn  # Skill 파일 작성 및 저장
```

**세션 로그 패턴:**
Pattern: `~/.claude/sessions/YYYY-MM-DD-topic.tmp`

예시: https://github.com/affaan-m/everything-claude-code/tree/main/examples/sessions/

### 다른 자체 개선 메모리 패턴

**RLanceMartin 접근법:**
- Session logs 리뷰
- 사용자 선호도 추출 (무엇이 작동하고 무엇이 작동하지 않는지)
- Session 후 "diary" 업데이트

**alexhillman 접근법:**
- 15분마다 proactively 개선 제안
- 최근 상호작용 리뷰
- 메모리 업데이트 제안
- 시간 경과에 따라 승인 패턴에서 학습

---

## 💰 Token Optimization

### 주요 전략: Subagent Architecture

가장 효과적인 방법은 올바른 모델을 올바른 작업에 위임하는 것입니다.

```
Haiku  ← 반복적 작업
  ↓
Sonnet ← 대부분의 코딩 (기본값)
  ↓
Opus   ← 복잡한 작업
```

### 벤치마킹 접근 방식 (더 정확)

1. 여러 git worktree 생성
2. 각각에 다른 모델의 subagent 할당
3. 동일한 작업 실행
4. 결과 비교 (테스트, 토큰 사용량, 품질)

### 모델 선택 빠른 참조

| 모델 | 비용 | 사용 사례 |
|------|------|----------|
| **Haiku** | $0.80/MTok | 반복적, 명확한 지침, worker |
| **Sonnet** | $3/MTok | 대부분의 코딩 (기본값) |
| **Opus** | $15/MTok | 복잡한 작업, 실패 후, 아키텍처 |

**Cost Ratio:**
- Sonnet vs Opus: 1.67배
- Haiku vs Opus: 5배

### 모델 선택 규칙

```yaml
---
name: quick-search
description: Fast file search
tools: [Glob, Grep]
model: haiku          # 저렴하고 빠름

---
name: refactoring
description: Complex refactoring
tools: [Read, Edit, Write]
model: sonnet         # 기본값

---
name: architecture-design
description: System design
tools: [Read, Bash]
model: opus           # 복잡한 결정
```

### Tool별 최적화

**mgrep > grep:**

기존 grep/ripgrep 대비 약 50% 토큰 감소

설치:
```bash
/plugins  # 마켓플레이스에서 찾기
```

또는:
```bash
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep
```

### 백그라운드 프로세스

Claude가 전체 출력을 처리할 필요가 없으면 Claude 외부에서 실행:

```bash
tmux new -s build
# npm run build (백그라운드에서)
# 필요한 부분만 복사하거나 요약
```

**이득:**
- 입력 토큰 절감 (대부분의 비용)
- 컨텍스트 유지

### 모듈식 코드베이스의 이점

**크기가 작은 파일 = 더 저렴 + 더 정확**

```
root/
├── docs/
├── scripts/
├── src/
│   ├── apps/               # Entry points
│   │   ├── api-gateway/
│   │   └── cron-jobs/
│   ├── modules/            # Core
│   │   ├── ordering/       # 자체 포함 모듈
│   │   │   ├── api/
│   │   │   ├── domain/
│   │   │   ├── infrastructure/
│   │   │   ├── use-cases/
│   │   │   └── tests/
│   │   ├── catalog/
│   │   └── identity/
│   ├── shared/             # 모든 모듈이 사용
│   │   ├── kernel/
│   │   ├── events/
│   │   └── utils/
│   └── main.ts
├── tests/                  # E2E tests
└── package.json
```

**이유:**
- Claude가 더 빠르게 읽음
- 중복 읽기 방지
- 더 저렴함

### System Prompt 최적화 (고급)

Claude Code의 system prompt: ~18k tokens (~9% of 200k)

줄일 수 있는 양: ~7,300 tokens 절감 (41% static overhead)

참조: YK's system-prompt-patches

---

## ✅ Verification Loops and Evals

### 관찰성 방법

**Option 1: tmux 프로세스 추적**
- Thinking stream 모니터링
- Skill trigger 시 output 캡처

**Option 2: PostToolUse Hook 로깅**
- Claude가 실행한 정확한 내용 로깅
- 변경 사항 및 출력 기록

### 벤치마킹 워크플로우

```
[Same Task]
  │
  ├────────────┬────────────┐
  ▼            ▼
┌──────────┐ ┌──────────┐
│Worktree A│ │Worktree B│
│WITH skill│ │NO skill  │
└────┬─────┘ └────┬─────┘
     │            │
     ▼            ▼
[Output A] [Output B]
     │            │
     └──────┬──────┘
          ▼
     [git diff]
          │
          ▼
┌─────────────────┐
│ Compare:        │
│ - logs          │
│ - token usage   │
│ - output quality│
└─────────────────┘
```

### Eval 패턴 유형

#### Checkpoint-Based Evals

```
[Task 1]
  │
  ▼
┌─────────┐
│Checkpoint│  ← verify criteria
│ #1      │
└────┬────┘
     │ pass?
     ├─yes─► [Task 2]
     └─no──► [Fix]
```

**최적:**
- Linear workflows
- Clear milestones

#### Continuous Evals

```
[Work]
  │
  ├─ Every N min or after change
  │
  ▼
[Run Tests + Lint]
  │
  ├─ Pass? ─yes─► [Continue]
  └─ Fail? ─no──► [Stop & Fix]
```

**최적:**
- Long sessions
- Exploratory refactoring
- No clear milestones

### Grader Types (Anthropic)

| 타입 | 방법 | 속도 | 비용 | 정확도 |
|------|------|------|------|--------|
| **Code-Based** | String match, tests, static analysis | 빠름 | 저 | 객관적 (but brittle) |
| **Model-Based** | Rubric scoring, natural language | 중간 | 중간 | 유연함 (but non-deterministic) |
| **Human** | Expert review, crowdsourced | 느림 | 고 | Gold standard |

### 핵심 메트릭

**pass@k:** At least ONE of k attempts succeeds

```
k=1: 70%  |  k=3: 91%  |  k=5: 97%
Higher k = higher odds of success
```

**pass^k:** ALL k attempts must succeed

```
k=1: 70%  |  k=3: 34%  |  k=5: 17%
Higher k = harder (consistency test)
```

**사용:**
- `pass@k`: 어떤 것이든 작동하면 됨
- `pass^k`: 일관성 필수 (near deterministic output)

### Eval Roadmap 구축

1. **Start early** - 실제 실패 사례 20-50개
2. **Convert failures** - User-reported issues → test cases
3. **Clear tasks** - 두 전문가가 같은 판단에 도달
4. **Balanced sets** - 해야 할 때와 하지 말아야 할 때 모두 포함
5. **Robust harness** - 각 시도는 clean environment에서
6. **Grade output** - 경로가 아닌 결과 평가
7. **Read transcripts** - 많은 시도의 기록 검토
8. **Monitor saturation** - 100% pass = 더 어려운 테스트 추가

---

## 🔀 Parallelization

### Fork 사용 시 체크리스트

When forking conversations in a multi-Claude terminal setup, make sure:

1. ✅ Scope이 잘 정의됨
2. ✅ 코드 변경 최소 overlap
3. ✅ 직교하는 작업 선택 (간섭 방지)

### 선호하는 패턴

**Main chat:** 코드 변경
**Forks:**
- 코드베이스 질문
- External services 연구
- 문서 검색
- GitHub repo 탐색

### 터미널 개수

❌ 임의로 많은 터미널 설정하지 마세요
✅ 진정한 필요와 목적으로만 추가

**목표:** Minimum viable parallelization으로 최대한 많이 할 수 있기

### Git Worktrees for Parallel

```bash
# Parallel work용 worktree 생성
git worktree add ../project-feature-a feature-a
git worktree add ../project-feature-b feature-b
git worktree add ../project-refactor refactor-branch

# 각 worktree는 자체 Claude instance 실행
cd ../project-feature-a && claude
```

**이점:**
- ✅ Git 충돌 없음
- ✅ 각각 clean working directory
- ✅ Output 비교 용이
- ✅ 다른 접근법 벤치마킹 가능

### Cascade Method

다중 Claude Code instance 운영할 때:

1. 새 작업을 오른쪽 탭에 열기
2. 왼쪽에서 오른쪽으로 스윕 (oldest to newest)
3. 일관된 방향 흐름 유지
4. **최대 3-4개 작업에만 집중** (그 이상은 오버헤드)

---

## 🏗️ Groundwork

When starting fresh, the actual foundation matters a lot.

### Two-Instance Kickoff Pattern

새로운 repo로 시작할 때 2개의 Claude instance 오픈:

#### Instance 1: Scaffolding Agent

```
→ Lay down scaffold and groundwork
→ Create project structure
→ Set up configs (CLAUDE.md, rules, agents)
→ Establish conventions
→ Skeleton in place
```

#### Instance 2: Deep Research Agent

```
→ Connect to all services, web search
→ Create detailed PRD
→ Create architecture mermaid diagrams
→ Compile references with actual clips
```

**Setup:** Left Terminal for Coding, Right Terminal for Questions
사용: `/rename` and `/fork`

### llms.txt Pattern

많은 문서 사이트에서 찾을 수 있습니다:

```bash
/llms.txt  # 문서 페이지에서 실행
```

예시: https://www.helius.dev/docs/llms.txt

**이점:**
- LLM-optimized 문서
- Claude에 바로 feed 가능
- Clean format

---

## 🧩 Philosophy: Build Reusable Patterns

### @omarsar0의 통찰

> "Early on, I spent time building reusable workflows/patterns. Tedious to build, but this had a wild compounding effect as models and agent harnesses improved."

### 투자해야 할 것

✅ Subagents (shorthand guide)
✅ Skills (shorthand guide)
✅ Commands (shorthand guide)
✅ Planning patterns
✅ MCP tools (shorthand guide)
✅ Context engineering patterns

### 왜 복합 효과가 나타날까?

> "The best part is that all these workflows are transferable to other agents like Codex. Once built, they work across model upgrades. **Investment in patterns > investment in specific model tricks.**"

---

## 🤖 Best Practices for Agents & Sub-Agents

### Sub-Agent Context 문제

Sub-agents exist to save context by returning summaries instead of dumping everything. But the orchestrator has semantic context the sub-agent lacks.

**Sub-agent의 한계:**
- Literal query만 알음
- Purpose/reasoning 모름
- Summaries often miss key details

### Analogy (@PerceptualPeak)

> "Your boss sends you to a meeting and asks for a summary. You come back and give him the rundown. Nine times out of ten, he's going to have follow-up questions. Your summary won't include everything he needs because you don't have the implicit context he has."

### 해결책: Iterative Retrieval Pattern

```
┌──────────────────┐
│  ORCHESTRATOR    │
│ (has context)    │
└────────┬─────────┘
         │ dispatch with query + objective
         ▼
┌──────────────────┐
│  SUB-AGENT       │
│ (lacks context)  │
└────────┬─────────┘
         │ returns summary
         ▼
┌──────────────────┐    ┌────────────┐
│  EVALUATE        │─no→│ FOLLOW-UP  │
│  Sufficient?     │    │ QUESTIONS  │
└────────┬─────────┘    └──────┬─────┘
         │ yes                 │
         ▼         sub-agent [ACCEPT]
    [ACCEPT]   fetches answers
                      ↑
                      └──────────────┘
               (max 3 cycles)
```

### Orchestrator 최적화

1. **Evaluate** 모든 sub-agent return
2. **Ask** 수용하기 전에 follow-up 질문
3. **Sub-agent** source로 돌아가 답변 획득
4. **Loop** 충분할 때까지 (max 3 cycles to prevent infinite loops)

### 팁: Objective Context 전달

단순히 query가 아니라 broader objective도 포함:

```
Query: "Find authentication patterns"
Objective: "Need for OAuth implementation in user module"
```

---

## 📋 Sequential Phase Pattern

```
Phase 1: RESEARCH
  │ (use Explore agent)
  │ - Gather context
  │ - Identify patterns
  └─→ Output: research-summary.md

Phase 2: PLAN
  │ (use planner agent)
  │ - Read research-summary.md
  │ - Create implementation plan
  └─→ Output: plan.md

Phase 3: IMPLEMENT
  │ (use tdd-guide agent)
  │ - Read plan.md
  │ - Write tests first
  │ - Implement code
  └─→ Output: code changes

Phase 4: REVIEW
  │ (use code-reviewer agent)
  │ - Review all changes
  └─→ Output: review-comments.md

Phase 5: VERIFY
  └─ (use build-error-resolver if needed)
    - Run tests
    - Fix issues
    └─→ Output: done or loop back
```

### 핵심 규칙

- ✅ 각 agent는 ONE 명확한 input과 ONE 명확한 output
- ✅ Outputs는 다음의 inputs이 됨
- ✅ Phase 건너뛰지 마세요 (각각 가치 추가)
- ✅ Agents 사이에 `/clear` 사용 (context 신선 유지)
- ✅ Intermediate outputs을 파일에 저장 (메모리만이 아님)

---

## 🎯 Agent Abstraction Tierlist (@menhguin)

### Tier 1: Direct Buffs (사용하기 쉬움)

**Subagents**
- Direct buff for preventing context rot
- Half as useful as multi-agent but MUCH less complexity

**Metaprompting**
- "I take 3 minutes to prompt a 20-minute task"
- Direct buff - improves stability and sanity-checks

**Asking user more at beginning**
- Generally a buff
- Plan mode에서 질문에 답해야 함

### Tier 2: High Skill Floor (사용하기 어려움)

**Long-running agents**
- Need to understand 15 min vs 1.5 hour vs 4 hour tradeoffs
- Takes tweaking and trial-and-error

**Parallel multi-agent**
- Very high variance
- Only useful on highly complex OR well-segmented tasks
- "If 2 tasks take 10 minutes and you spend arbitrary time prompting, it's counterproductive"

**Role-based multi-agent**
- "Models evolve too fast for hard-coded heuristics"
- Hard to test

**Computer use agents**
- Very early paradigm
- Requires wrangling

### 핵심 교훈

**Tier 1부터 시작.** Only graduate to Tier 2 when you've mastered the basics and have a genuine need.

---

## 💡 Tips and Tricks

### MCP 대체 가능성 (Context Window 절감)

Many MCPs (GitHub, Supabase, Vercel, Railway) are essentially wrappers around robust CLIs.

**대신 할 수 있는 것:**

```bash
# MCP 대신 CLI + Skills 조합
# 예: GitHub MCP 대신
/gh-pr  # Wraps 'gh pr create' with your options
```

**이점:**
- ✅ Same functionality
- ✅ Similar convenience
- ✅ **Freed up context window**

### Lazy Loading 개선 (최근)

Boris and Claude Code team이 recent improvements 수행:

- ✅ MCPs lazy loading (startup 시 context 소비 안 함)
- ❌ Token usage/cost는 여전히 이슈

**CLI + skills approach:** Token optimization에 여전히 효과적

---

## 🎬 Video Series?

위의 기술들을 End-to-end 프로젝트로 보여주는 비디오 제작 계획:

### 커버할 내용

✅ Full project setup with configs (shorthand guide)
✅ Advanced techniques in action (longform guide)
✅ Real-time token optimization
✅ Verification loops in practice
✅ Memory management across sessions
✅ Two-instance kickoff pattern
✅ Parallel workflows with git worktrees
✅ Screenshots and recordings of actual workflow

---

## 📚 References

- [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) (Jan 2026)
- Anthropic: "Claude Code Best Practices" (Apr 2025)
- Fireworks AI: "Eval Driven Development with Claude Code" (Aug 2025)
- [YK: 32 Claude Code Tips](https://agenticcoding.substack.com/p/32-claude-code-tips-from-basics-to) (Dec 2025)
- Addy Osmani: "My LLM coding workflow going into 2026"
- @PerceptualPeak: Sub-Agent Context Negotiation
- @menhguin: Agent Abstractions Tierlist
- @omarsar0: Compound Effects Philosophy
- [RLanceMartin: Session Reflection Pattern](https://rlancemartin.github.io/2025/12/01/claude_diary/)
- @alexhillman: Self-Improving Memory System

---

**GitHub:** https://github.com/affaan-m/everything-claude-code

---

*이 노트는 X의 게시물을 마크다운 형식으로 변환하여 저장되었습니다.*
