---
title: "phuryn/pm-skills: PM Skills Marketplace: 100+ agentic skills, commands, and plugins — from discovery to strategy, execution, launch, and growth."
source: "https://github.com/phuryn/pm-skills"
author:
published:
created: 2026-04-24
description: "PM Skills Marketplace: 100+ agentic skills, commands, and plugins — from discovery to strategy, execution, launch, and growth. - phuryn/pm-skills"
tags:
  - "clippings"
---
## PM 스킬 마켓플레이스: 더 나은 제품 결정을 위한 AI 운영 체제

> 8개 플러그인에 걸친 65개의 PM 스킬과 36개의 연쇄 워크플로우. Claude Code, Cowork 등. 발견부터 전략, 실행, 출시, 성장까지.

[![Plugin overview](https://github.com/phuryn/pm-skills/raw/main/.docs/images/plugins-overview.webp)](https://github.com/phuryn/pm-skills/blob/main/.docs/images/plugins-overview.webp)

Claude Code와 Cowork를 위해 설계됨. 다른 AI 어시스턴트와 호환되는 스킬.

## 여기서 시작하기

새로운 아이디어? → `/discover`  
전략적 명확성이 필요한가요? → `/strategy`  
PRD를 작성 중인가요? → `/write-prd`  
출시를 계획 중인가요? → `/plan-launch`  
메트릭 정의하기? → `/north-star`

이 프로젝트가 도움이 되었다면 레포에 ⭐를 눌러주세요.

## PM Skills Marketplace를 사용하는 이유는?

일반적인 AI는 텍스트를 제공합니다. PM Skills Marketplace는 구조를 제공합니다.

각 스킬은 검증된 PM 프레임워크(발견, 가정 매핑, 우선순위 지정, 전략)를 인코딩하고 단계별로 안내합니다. Teresa Torres, Marty Cagan, Alberto Savoia의 엄격함이 책장에 앉아있는 것이 아니라 일일 워크플로우에 내장되어 있습니다.

결과: 더 빠른 문서가 아닌 더 나은 제품 결정.

## 작동 방식 (스킬, 커맨드, 플러그인)

**스킬** 은 마켓플레이스의 기본 구성 요소입니다. 각 스킬은 Claude에 특정 PM 작업을 위한 도메인 지식, 분석 프레임워크 또는 안내식 워크플로우를 제공합니다. 일부 스킬은 여러 커맨드가 공유하는 재사용 가능한 기초로도 작동합니다.

스킬은 대화와 관련이 있을 때 자동으로 로드되며, 명시적인 호출이 필요하지 않습니다. 필요한 경우(예: 일반 지식보다 스킬을 우선시할 때) `/plugin-name:skill-name` 또는 `/skill-name` 으로 스킬 로드를 강제할 수 있습니다(Claude가 접두사를 추가합니다).

**명령어** 는 `/command-name` 으로 호출되는 사용자 트리거 워크플로우입니다. 하나 이상의 스킬을 엔드-투-엔드 프로세스로 연결합니다. 예를 들어, `/discover` 는 네 가지 스킬을 연결합니다: brainstorm-ideas → identify-assumptions → prioritize-assumptions → brainstorm-experiments.

**플러그인** 은 관련 스킬과 명령어를 설치 가능한 패키지로 그룹화합니다. 각 플러그인은 PM 도메인(discovery, strategy, execution 등)을 다룹니다. 마켓플레이스를 설치하면 8개의 플러그인을 모두 한 번에 얻을 수 있습니다.

[![How skills work](https://github.com/phuryn/pm-skills/raw/main/.docs/images/how-skills-work.webp)](https://github.com/phuryn/pm-skills/blob/main/.docs/images/how-skills-work.webp)

명령어는 스킬을 사용합니다. 일부 스킬은 여러 명령어를 지원합니다. 일부 스킬(예: `prioritization-frameworks` 또는 `opportunity-solution-tree`)은 Claude가 필요할 때마다 활용하는 독립형 참고 자료이며, 명령어가 필요하지 않습니다.

명령어들은 PM 워크플로우와 일치하도록 서로 연결되도록 설계되었습니다. 모든 명령어가 완료된 후, 관련된 다음 명령어들을 제안합니다 — 프롬프트를 따르기만 하면 됩니다.

## 설치

### Claude Cowork (개발자가 아닌 경우 권장)

1. **Customize** (왼쪽 하단)를 엽니다
2. **Browse plugins** → **Personal** → **+로 이동합니다**
3. **GitHub에서 마켓플레이스 추가 선택**
4. 입력: `phuryn/pm-skills`

모든 8개 플러그인이 자동으로 설치됩니다. 명령어(`/discover`, `/strategy` 등)와 스킬을 모두 얻게 됩니다.

[![Installing PM Skills in Claude Cowork](https://github.com/phuryn/pm-skills/raw/main/.docs/images/pm-skills-install.gif)](https://github.com/phuryn/pm-skills/blob/main/.docs/images/pm-skills-install.gif)

### Claude 코드 (CLI)

```
# Step 1: Add the marketplace
claude plugin marketplace add phuryn/pm-skills

# Step 2: Install individual plugins
claude plugin install pm-toolkit@pm-skills
claude plugin install pm-product-strategy@pm-skills
claude plugin install pm-product-discovery@pm-skills 
claude plugin install pm-market-research@pm-skills 
claude plugin install pm-data-analytics@pm-skills
claude plugin install pm-marketing-growth@pm-skills
claude plugin install pm-go-to-market@pm-skills
claude plugin install pm-execution@pm-skills
```

### 다른 AI 어시스턴트 (스킬만 해당)

`skills/*/SKILL.md` 파일은 범용 스킬 형식을 따르며 이를 읽는 모든 도구와 호환됩니다. 명령어(`/slash-commands`)는 Claude 전용입니다.

| 도구 | 사용 방법 | 작동하는 것 |
| --- | --- | --- |
| **Gemini CLI** | `.gemini/skills/에 스킬 폴더 복사` | 스킬만 |
| **OpenCode** | `.opencode/skills/에 스킬 폴더 복사` | 스킬만 |
| **Cursor** | 스킬 폴더를 `.cursor/skills/로 복사` | 스킬만 |
| **Codex CLI** | 스킬 폴더를 `.codex/skills/로 복사` | 스킬만 해당 |
| **키로** | 스킬 폴더를 `.kiro/skills/로 복사` | 스킬만 |

```
# Example: copy all skills for OpenCode (project-level)
for plugin in pm-*/; do
  mkdir -p .opencode/skills/
  cp -r "$plugin/skills/"* .opencode/skills/ 2>/dev/null
done

# Example: copy all skills for Gemini CLI (global)
for plugin in pm-*/; do
  cp -r "$plugin/skills/"* ~/.gemini/skills/ 2>/dev/null
done
```

---

## 사용 가능한 플러그인

**1\. pm-product-discovery** — 아이디에이션, 실험, 가정 검증, OST, 인터뷰 (13개 스킬, 5개 커맨드)

**Skills (13):**

- `brainstorm-ideas-existing` — Multi-perspective ideation for existing products (PM, Designer, Engineer)
- `brainstorm-ideas-new` — Ideation for new products in initial discovery
- `brainstorm-experiments-existing` — Design experiments to test assumptions for existing products
- `brainstorm-experiments-new` — Design lean startup pretotypes for new products (Alberto Savoia)
- `identify-assumptions-existing` — Identify risky assumptions across Value, Usability, Viability, and Feasibility
- `identify-assumptions-new` — Identify risky assumptions across 8 risk categories including Go-to-Market, Strategy, and Team
- `prioritize-assumptions` — Prioritize assumptions using an Impact × Risk matrix with experiment suggestions
- `prioritize-features` — Prioritize a feature backlog based on impact, effort, risk, and strategic alignment
- `analyze-feature-requests` — Analyze and categorize customer feature requests by theme and strategic fit
- `opportunity-solution-tree` — Build an Opportunity Solution Tree (Teresa Torres) — outcome → opportunities → solutions → experiments
- `interview-script` — Create a structured customer interview script with JTBD probing questions
- `summarize-interview` — Summarize an interview transcript into JTBD, satisfaction signals, and action items
- `metrics-dashboard` — Design a product metrics dashboard with North Star, input metrics, and alert thresholds

**Commands (5):**

- `/discover` — Full discovery cycle: ideation → assumption mapping → prioritization → experiment design
- `/brainstorm` — Multi-perspective ideation (`ideas|experiments` × `existing|new`)
- `/triage-requests` — Analyze and prioritize a batch of feature requests
- `/interview` — Prepare an interview script or summarize a transcript (`prep|summarize`)
- `/setup-metrics` — Design a product metrics dashboard

**Examples:**

Skills:

- `What are the riskiest assumptions for our AI writing assistant idea?`
- `Help me build an Opportunity Solution Tree for improving user activation`
- `Prioritize these 12 feature requests from our enterprise customers [attach CSV]`

Commands:

- `/discover AI-powered meeting summarizer for remote teams`
- `/brainstorm experiments existing — We need to reduce churn in our onboarding flow`
- `/interview prep — We're interviewing enterprise buyers about their procurement workflow`
**2\. pm-product-strategy** — 비전, 비즈니스 모델, 가격 책정, 경쟁 환경 (12개 스킬, 5개 커맨드)

Product strategy, vision, business models, pricing, and macro environment analysis. Covers the full strategic toolkit from vision crafting through competitive landscape scanning.

**Skills (12):**

- `product-strategy` — Comprehensive 9-section Product Strategy Canvas (vision → defensibility)
- `startup-canvas` — Startup Canvas combining Product Strategy (9 sections) + Business Model — an alternative to BMC and Lean Canvas for new products
- `product-vision` — Craft an inspiring, achievable, and emotional product vision
- `value-proposition` — 6-part JTBD value proposition (Who, Why, What before, How, What after, Alternatives)
- `lean-canvas` — Lean Canvas business model for startups and new products
- `business-model` — Business Model Canvas with all 9 building blocks
- `monetization-strategy` — Brainstorm 3–5 monetization strategies with validation experiments
- `pricing-strategy` — Pricing models, competitive analysis, willingness-to-pay, and price elasticity
- `swot-analysis` — SWOT analysis with actionable recommendations
- `pestle-analysis` — Macro environment: Political, Economic, Social, Technological, Legal, Environmental
- `porters-five-forces` — Competitive forces analysis (rivalry, suppliers, buyers, substitutes, new entrants)
- `ansoff-matrix` — Growth strategy mapping across markets and products

**Commands (5):**

- `/strategy` — Create a complete 9-section Product Strategy Canvas
- `/business-model` — Explore business models (`lean|full|startup|value-prop|all`)
- `/value-proposition` — Design a value proposition using the 6-part JTBD template
- `/market-scan` — Macro environment analysis combining SWOT + PESTLE + Porter's + Ansoff
- `/pricing` — Design a pricing strategy with competitive analysis and experiments

**Examples:**

Skills:

- `Compare Lean Canvas vs Business Model Canvas vs Startup Canvas for my marketplace startup`
- `Design a value proposition for our AI writing assistant targeting non-native English speakers`
- `Run a Porter's Five Forces analysis for the project management SaaS market`

Commands:

- `/strategy B2B project management tool for agencies`
- `/business-model startup — AI writing tool for non-native English speakers`
- `/value-proposition SaaS onboarding tool for enterprise customers`
**3\. pm-execution** — PRD, OKR, 로드맵, 스프린트, 회고, 릴리스 노트, 이해관계자 관리 (15개 스킬, 10개 커맨드)

Day-to-day product management: PRDs, OKRs, roadmaps, sprints, retrospectives, release notes, pre-mortems, stakeholder management, user stories, and prioritization frameworks.

**Skills (15):**

- `create-prd` — Comprehensive 8-section PRD template
- `brainstorm-okrs` — Team-level OKRs aligned with company objectives
- `outcome-roadmap` — Transform a feature list into an outcome-focused roadmap
- `sprint-plan` — Sprint planning with capacity estimation, story selection, and risk identification
- `retro` — Structured sprint retrospective facilitation
- `release-notes` — User-facing release notes from tickets, PRDs, or changelogs
- `pre-mortem` — Risk analysis with Tigers/Paper Tigers/Elephants classification
- `stakeholder-map` — Power × Interest grid with tailored communication plan
- `summarize-meeting` — Meeting transcript → decisions + action items
- `user-stories` — User stories following the 3 C's and INVEST criteria
- `job-stories` — Job stories: When \[situation\], I want to \[motivation\], so I can \[outcome\]
- `wwas` — Product backlog items in Why-What-Acceptance format
- `test-scenarios` — Test scenarios: happy paths, edge cases, error handling
- `dummy-dataset` — Realistic dummy datasets as CSV, JSON, SQL, or Python
- `prioritization-frameworks` — Reference guide to 9 prioritization frameworks (Opportunity Score, ICE, RICE, MoSCoW, Kano, etc.)

**Commands (10):**

- `/write-prd` — Create a PRD from a feature idea or problem statement
- `/plan-okrs` — Brainstorm team-level OKRs
- `/transform-roadmap` — Convert a feature-based roadmap into outcome-focused
- `/sprint` — Sprint lifecycle (`plan|retro|release`)
- `/pre-mortem` — Pre-mortem risk analysis on a PRD or launch plan
- `/meeting-notes` — Summarize a meeting transcript into structured notes
- `/stakeholder-map` — Map stakeholders and create a communication plan
- `/write-stories` — Break features into backlog items (`user|job|wwa`)
- `/test-scenarios` — Generate test scenarios from user stories
- `/generate-data` — Create realistic dummy datasets

**Examples:**

Skills:

- `Which prioritization framework should I use for a 50-item backlog?`
- `Map our stakeholders for the platform migration project`
- `What's the difference between Opportunity Score, ICE, and RICE?`

Commands:

- `/write-prd Smart notification system that reduces alert fatigue`
- `/sprint retro — Here are the notes from our last sprint`
- `/write-stories job — Break down the "team dashboard" feature into job stories`
**4\. pm-market-research** — 페르소나, 세분화, 여정 맵, 시장 규모 산정, 경쟁사 분석 (7개 스킬, 3개 커맨드)

User research and competitive analysis: personas, segmentation, journey maps, market sizing, competitor analysis, and feedback analysis.

**Skills (7):**

- `user-personas` — Create refined user personas from research data
- `market-segments` — Identify 3–5 customer segments with demographics, JTBD, and product fit
- `user-segmentation` — Segment users from feedback data based on behavior, JTBD, and needs
- `customer-journey-map` — End-to-end journey map with stages, touchpoints, emotions, and pain points
- `market-sizing` — TAM, SAM, SOM with top-down and bottom-up approaches
- `competitor-analysis` — Competitor strengths, weaknesses, and differentiation opportunities
- `sentiment-analysis` — Sentiment analysis and theme extraction from user feedback

**Commands (3):**

- `/research-users` — Build personas, segment users, and map the customer journey
- `/competitive-analysis` — Analyze the competitive landscape
- `/analyze-feedback` — Sentiment analysis and segment insights from user feedback

**Examples:**

Skills:

- `Estimate TAM/SAM/SOM for an AI code review tool in the US market`
- `Create a customer journey map for our e-commerce checkout flow`
- `Segment these survey respondents by behavior and needs [attach CSV]`

Commands:

- `/research-users We have interview data from 12 users of our fitness app`
- `/competitive-analysis Figma competitors in the design tool space`
- `/analyze-feedback Here's 200 NPS responses from Q4 [attach file]`
**5\. pm-data-analytics** — SQL 생성, 코호트 분석, A/B 테스트 분석 (3개 스킬, 3개 커맨드)

Data analytics for PMs: SQL query generation, cohort analysis, and A/B test analysis.

**Skills (3):**

- `sql-queries` — Generate SQL from natural language (BigQuery, PostgreSQL, MySQL)
- `cohort-analysis` — Retention curves, feature adoption, and engagement trends by cohort
- `ab-test-analysis` — Statistical significance, sample size validation, and ship/extend/stop recommendations

**Commands (3):**

- `/write-query` — Generate SQL queries from natural language
- `/analyze-cohorts` — Cohort analysis on user engagement data
- `/analyze-test` — Analyze A/B test results

**Examples:**

Skills:

- `How large a sample do I need for 95% confidence with a 2% MDE?`
- `What retention metrics should I track for a subscription app?`

Commands:

- `/write-query Show me monthly active users by country for Q4 2025 (BigQuery)`
- `/analyze-test Here are the results from our checkout flow A/B test [attach CSV]`
- `/analyze-cohorts Weekly retention for users who signed up in January vs February`
**6\. pm-go-to-market** — 비치헤드 세그먼트, ICP, 메시징, 성장 루프, GTM 모션, 배틀카드 (6개 스킬, 3개 커맨드)

Go-to-market strategy: beachhead segments, ideal customer profiles, messaging, growth loops, GTM motions, and competitive battlecards.

**Skills (6):**

- `gtm-strategy` — Full GTM strategy: channels, messaging, success metrics, and launch plan
- `beachhead-segment` — Identify the first beachhead market segment
- `ideal-customer-profile` — ICP with demographics, behaviors, JTBD, and needs
- `growth-loops` — Design sustainable growth loops (flywheels)
- `gtm-motions` — Evaluate GTM motions and tools (product-led, sales-led, etc.)
- `competitive-battlecard` — Sales-ready battlecard with objection handling and win strategies

**Commands (3):**

- `/plan-launch` — Full GTM strategy from beachhead to launch plan
- `/growth-strategy` — Design growth loops and evaluate GTM motions
- `/battlecard` — Create a competitive battlecard

**Examples:**

Skills:

- `What's the best beachhead segment for a developer productivity tool?`
- `Design a growth loop for a B2B SaaS with a freemium tier`
- `Define our ICP for an AI-powered HR screening platform`

Commands:

- `/plan-launch AI code review tool targeting mid-size engineering teams`
- `/battlecard Our CRM vs Salesforce for the SMB market`
- `/growth-strategy Two-sided marketplace for connecting freelancers with startups`
**7\. pm-marketing-growth** — 마케팅 아이디어, 포지셔닝, 가치 제안, 네이밍, 노스 스타 메트릭 (5개 스킬, 2개 커맨드)

Product marketing and growth: marketing ideas, positioning, value proposition statements, product naming, and North Star metrics.

**Skills (5):**

- `marketing-ideas` — Creative, cost-effective marketing ideas with channels and messaging
- `positioning-ideas` — Product positioning differentiated from competitors
- `value-prop-statements` — Value proposition statements for marketing, sales, and onboarding
- `product-name` — Product name brainstorming aligned to brand values and audience
- `north-star-metric` — North Star Metric + input metrics with business game classification

**Commands (2):**

- `/market-product` — Brainstorm marketing ideas, positioning, value props, and product names
- `/north-star` — Define your North Star Metric and supporting input metrics

**Examples:**

Skills:

- `Brainstorm 5 positioning angles that differentiate us from Notion`
- `What's a good North Star Metric for a two-sided marketplace?`
- `Generate value prop statements for our sales team's pitch deck`

Commands:

- `/market-product B2B analytics dashboard for e-commerce managers`
- `/north-star Two-sided marketplace connecting freelancers with clients`
**8\. pm-toolkit** — 이력서 검토, 법률 문서, 교정 (4개 스킬, 5개 커맨드)

PM utilities beyond core product work: resume review, legal documents, and proofreading.

**Skills (4):**

- `review-resume` — PM resume review and tailoring against 10 best practices (XYZ+S formula, keywords, structure)
- `draft-nda` — Non-Disclosure Agreement with jurisdiction-appropriate clauses
- `privacy-policy` — Privacy policy covering GDPR/CCPA compliance
- `grammar-check` — Grammar, logic, and flow checking with targeted fixes

**Commands (5):**

- `/review-resume` — Comprehensive PM resume review
- `/tailor-resume` — Tailor a resume to a specific job description
- `/draft-nda` — Draft an NDA
- `/privacy-policy` — Draft a privacy policy
- `/proofread` — Check grammar, logic, and flow

**Examples:**

Skills:

- `Review my PM resume against best practices [attach PDF]`
- `Check this product announcement for grammar and clarity`

Commands:

- `/review-resume [attach your PM resume]`
- `/tailor-resume [attach resume + paste job description]`
- `/proofread Here's the draft of our Q1 investor update`

---

## 소개

이 마켓플레이스는 제품 관행과 AI 역량에 따라 진화합니다.

다음의 작업을 기반으로 선택된 스킬:

- Teresa Torres — [*지속적 발견 습관*](https://www.amazon.com/Continuous-Discovery-Habits-Discover-Products/dp/1736633309/)
- Marty Cagan — [*영감*](https://www.amazon.com/INSPIRED-Create-Tech-Products-Customers/dp/1119387507/) 과 [*변혁*](https://www.amazon.com/dp/1119697336/)
- Alberto Savoia — [*The Right It*](https://www.amazon.com/Right-Many-Ideas-Yours-Succeed/dp/0062884654)
- Dan Olsen — [*The Lean Product Playbook*](https://www.amazon.com/dp/1118960874/)
- Roger L. Martin — [*Playing to Win*](https://www.amazon.com/Playing-Win-Expanded-Bonus-Articles/dp/B0F25SDYWV/)
- Ash Maurya — [*Running Lean*](https://www.amazon.com/dp/B004J4XGN6/)
- Strategyzer — [*Business Model Generation*](https://www.amazon.com/dp/0470876417/) 및 [*Value Proposition Design*](https://www.amazon.com/dp/1118968050/)
- Christina Wodtke — [*Radical Focus*](https://www.amazon.com/Radical-Focus-Achieving-Important-Objectives/dp/0996006052)
- Anthony W. Ulwick — [*Jobs to Be Done*](https://jobs-to-be-done-book.com/)
- Alistair Croll & Benjamin Yoskovitz — [*Lean Analytics*](https://www.amazon.com/Lean-Analytics-Better-Startup-Faster/dp/1449335675/)
- Sean Ellis — [*Hacking Growth*](https://www.amazon.com/Hacking-Growth-Fastest-Growing-Companies-Breakout/dp/045149721X/)
- Maja Voje — [*Go-To-Market Strategist*](https://gtmstrategist.com/)

Paweł Huryn이 [The Product Compass Newsletter](https://www.productcompass.pm/) 에서 엄선함.

## 기여하기

[CONTRIBUTING.md](https://github.com/phuryn/pm-skills/blob/main/CONTRIBUTING.md) 를 참조하세요.

## Windows의 알려진 문제

Cowork가 불안정하고 VM을 시작할 수 없는 경우([claude-code/issues/27010](https://github.com/anthropics/claude-code/issues/27010)), 다음을 시도하세요:

```
$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-WindowStyle Hidden -Command \`"if ((Get-Service CoworkVMService).Status -ne 'Running') { Start-Service CoworkVMService }\`""

$trigger = New-ScheduledTaskTrigger -RepetitionInterval (New-TimeSpan -Minutes 1) -Once -At (Get-Date)

$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries

Register-ScheduledTask -TaskName "CoworkVMServiceMonitor" \`
  -Action $action \`
  -Trigger $trigger \`
  -Settings $settings \`
  -RunLevel Highest \`
  -User "SYSTEM"
```

Windows의 90%의 문제를 해결합니다. 남은 10%: services.msc를 열고 > "Claude" 서비스를 수동으로 시작하세요