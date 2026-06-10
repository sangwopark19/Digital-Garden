---
title: "Post by @choi.openai on Threads"
source: "https://www.threads.com/@choi.openai/post/DYtJ60rD-dq"
author:
  - "[[@choi.openai]]"
published: 2026-05-24
created: 2026-06-10
description: "2026-05-24"
tags:
  - "clippings"
---
대규모 코드베이스에서 AI 코딩을 제대로 쓰는 법이 드러났습니다.

앤트로픽은 Claude Code가 수백만 줄짜리 모노레포와 오래된 레거시 시스템에서도 실제로 쓰이고 있다고 밝혔습니다.

핵심은 모델 성능보다 [CLAUDE.md](http://claude.md/), hooks, skills, plugins, LSP, MCP를 어떻게 묶느냐였습니다. 실무 적용 사례와 설정법을 정리했습니다🧵 

> **@choi.openai**
> 
> 2026-05-24
> 
> [https://www.threads.com/@choi.openai/post/DYtJ60rD-dq](https://www.threads.com/@choi.openai/post/DYtJ60rD-dq)

---

## Comments

> **@choi.openai** · [2026-05-24](https://www.threads.com/@choi.openai/post/DYtJ7wSD-YT)
> 
> 1/ 2/ 성공적인 도입을 위해서는 AI 모델 자체의 성능보다 모델을 둘러싼 '하네스(Harness, 확장 계층)' 구성이 훨씬 중요합니다.
> 
> 첫 단추는 [CLAUDE.md](http://claude.md/) 파일입니다. 프로젝트의 규칙과 아키텍처를 담은 이 파일을 디렉터리별로 가볍게 배치해야 합니다.
> 
> 다음은 훅(Hooks)과 스킬(Skills)의 활용입니다. 훅은 AI의 세션 결과를 반영해 컨텍스트를 자동 업데이트하거나 린팅(Linting) 규칙을 강제하여 일관성을 높입니다.
> 
> 스킬은 보안 검토나 문서화 등 특정 작업에 필요한 전문 지식을 필요할 때만 호출(Progressive disclosure)하여, 귀중한 컨텍스트 창이 불필요한 정보로 낭비되는 것을 막아주는 핵심 최적화 도구입니다. 
> 
> ![Photo by CHOI on May 23, 2026. May be a graphic of crossword puzzle, calendar, poster and text that says 'The Claude TheClaudeCodeharness Code harness Start CLAUDE.md Foundation Session Sessiontime time Hooks Self-improvement End Start Skills Progressive disclosure Plugins Distribution End LSP Navigation Installed pre-session hntllopresssalaltot -availablothroughout available throughout MCPservers MCP servers Extension Subagents Exploration editing ···: Map findings Map→TindingsTile rile'.](https://scontent-icn2-1.cdninstagram.com/v/t51.82787-15/705320620_17967100302112832_4969962306686639264_n.jpg?stp=dst-jpg_e35_tt6&_nc_cat=101&ig_cache_key=MzkwMzgyMDE0MDcxMDQ1NDgwMw%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkZFRUQueHBpZHMuMjg4MC5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=0PJqHMSvyT8Q7kNvwE6-w44&_nc_oc=Adp-pi7yoNRMFyiwTTy1DMkPk1oMXVoB3PTP_uh1GhSPg3o9uggECk2BFLonGpSl0QY&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-icn2-1.cdninstagram.com&_nc_gid=xO7rbbq7OghR1ADLhm4Xmw&_nc_ss=7a22e&oh=00_Af-XARHKkjleAc-LzVZ-yXKj0zqnmmt0EGi10C0YgaKrLw&oe=6A2E6408)
> 
> > **@choi.openai**
> > 
> > 2026-05-24
> > 
> > [https://www.threads.com/@choi.openai/post/DYtJ7wSD-YT](https://www.threads.com/@choi.openai/post/DYtJ7wSD-YT)
> 
> > **@choi.openai** · [2026-05-24](https://www.threads.com/@choi.openai/post/DYtJ8-kj4NI)
> > 
> > 2/ 개인 단위의 성공적인 설정을 조직 전체로 퍼뜨리는 도구들도 필수적입니다.
> > 
> > 플러그인(Plugins)은 잘 짜인 스킬과 훅, 설정값들을 하나의 패키지로 묶어 배포하므로, 신규 입사자도 설치 첫날부터 완벽한 AI 환경을 누릴 수 있습니다.
> > 
> > 또한, 거대 코드베이스에서 가장 빛을 발하는 것은 LSP(언어 서버 프로토콜) 연동입니다.
> > 
> > 단순한 텍스트 검색(Grep)은 수천 개의 무의미한 결과로 컨텍스트를 낭비하지만, LSP를 연동하면 기호(Symbol) 단위의 정밀한 탐색이 가능해져 C++나 Java 같은 복잡한 언어에서도 "정의로 이동" 기능을 사람처럼 정확히 수행합니다.
> > 
> > 여기에 사내 도구 및 데이터와 연결해 주는 MCP 서버까지 결합하면 활용도는 극대화됩니다. 
> > 
> > > **@choi.openai**
> > > 
> > > 2026-05-24
> > > 
> > > [https://www.threads.com/@choi.openai/post/DYtJ8-kj4NI](https://www.threads.com/@choi.openai/post/DYtJ8-kj4NI)
> > 
> > > **@choi.openai** · [2026-05-24](https://www.threads.com/@choi.openai/post/DYtJ-DPj84v)
> > > 
> > > 3/ 코드베이스 탐색 효율을 극대화하기 위한 구체적인 설정 패턴도 존재합니다.
> > > 
> > > 루트 디렉터리에 모든 정보를 때려 넣는 대신, 하위 디렉터리 단위로 초기화하여 AI가 관련 없는 영역에서 헤매지 않게 해야 합니다.
> > > 
> > > 테스트와 빌드 명령어도 해당 디렉터리에 맞게 제한해야 타임아웃을 막을 수 있습니다.
> > > 
> > > 빌드 산출물이나 외부 라이브러리는 .ignore 파일로 철저히 배제하여 노이즈를 줄여야 합니다.
> > > 
> > > 또한 복잡한 시스템에서는 메인 에이전트가 직접 탐색과 수정을 동시에 하기보다, '서브에이전트(Subagents)'를 띄워 특정 하위 시스템 구조만 파악해 파일로 요약하게 한 뒤, 메인 에이전트가 그 요약본을 보고 전체 코드를 수정하는 역할 분담이 훨씬 효율적입니다. 
> > > 
> > > ![Photo by CHOI on May 23, 2026. May be an illustration of crossword puzzle, poster and text.](https://scontent-icn2-1.cdninstagram.com/v/t51.82787-15/704714980_17967100461112832_4860005665694051545_n.jpg?stp=dst-jpg_e35_tt6&_nc_cat=104&ig_cache_key=MzkwMzgyMDI5ODUwODU1Mzc3NQ%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkZFRUQueHBpZHMuMjg4MC5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=aLG67p68N6sQ7kNvwHDHKYe&_nc_oc=Adof33oi5iOQnxMqrhjg1B61QlCuZNG-cWkg0FGjHJ7qFI6aueFg7waXwH8K5Irj4gI&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-icn2-1.cdninstagram.com&_nc_gid=xO7rbbq7OghR1ADLhm4Xmw&_nc_ss=7a22e&oh=00_Af8i8ZbKt0pwJOylATkXfp3mvj13ftFNLyrfKVPfKdYREQ&oe=6A2E7388)
> > > 
> > > > **@choi.openai**
> > > > 
> > > > 2026-05-24
> > > > 
> > > > [https://www.threads.com/@choi.openai/post/DYtJ-DPj84v](https://www.threads.com/@choi.openai/post/DYtJ-DPj84v)
> > > 
> > > > **@choi.openai** · [2026-05-24](https://www.threads.com/@choi.openai/post/DYtJ_dzjwLw)
> > > > 
> > > > 4/ 마지막으로 가장 간과하기 쉬운 것은 '조직적 관리와 최신화'입니다.
> > > > 
> > > > 새로운 AI 모델이 출시되어 추론 능력이 좋아지면, 과거에 AI의 실수를 막기 위해 만들어둔 촘촘한 규칙([CLAUDE.md](http://claude.md/))이나 방어적인 훅(Hooks)들이 오히려 새 모델의 발목을 잡는 제약이 될 수 있습니다.
> > > > 
> > > > 따라서 3~6개월 주기로 설정값을 대대적으로 재검토해야 합니다.
> > > > 
> > > > 이를 위해 바텀업 방식의 자율에만 맡기지 말고, 개발자 경험(DX) 팀 산하에 '에이전트 매니저(Agent Manager)'나 전담 책임자(DRI)를 두어야 합니다.
> > > > 
> > > > 이들이 모범 사례를 중앙에서 표준화하고 사내 플러그인 생태계를 관리해야만 거대 조직에서 파편화 없이 AI 도입의 진짜 가치를 뽑아낼 수 있습니다. 
> > > > 
> > > > ![Photo by CHOI on May 23, 2026. May be a graphic of crossword puzzle and text that says 'Three phases of a Claude Code rollout Phase 01 Quiet investment Before broadaccess broad access Phase02 02 The rollout lands Day one Phase 03 Adoption spreads After rollout Ownersassemble Owners assemble infrastructure piece by piece. Infrastructure is s ready. First wave developers findsi it productive. Wordmovesteamtoteam. Word moves team to team. population Thepopulationonthe on the harness grows.'.](https://scontent-icn2-1.cdninstagram.com/v/t51.82787-15/705350382_17967100470112832_8609077995394053216_n.jpg?stp=dst-jpg_e35_tt6&_nc_cat=110&ig_cache_key=MzkwMzgyMDM5NTc0OTI0NTY4MA%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkZFRUQueHBpZHMuMjg4MC5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=DhpjrWEpyTUQ7kNvwHlTlAq&_nc_oc=Adrbk-5IyKCG2BMzyOtgwwpAWxy4TMFsc_70eaznBZiISMlhoWh9uWSE08MLBDeRMGA&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-icn2-1.cdninstagram.com&_nc_gid=xO7rbbq7OghR1ADLhm4Xmw&_nc_ss=7a22e&oh=00_Af82SzCj-JhmQI0UQroV-8V2CZ7uxCsUgATpwAdZ2A3sKQ&oe=6A2E9392)
> > > > 
> > > > > **@choi.openai**
> > > > > 
> > > > > 2026-05-24
> > > > > 
> > > > > [https://www.threads.com/@choi.openai/post/DYtJ\_dzjwLw](https://www.threads.com/@choi.openai/post/DYtJ_dzjwLw)
> > > > 
> > > > > **@choi.openai** · [2026-05-24](https://www.threads.com/@choi.openai/post/DYtKAnoD0-S)
> > > > > 
> > > > > 전체 내용 : [claude.com/blog…](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start) 
> > > > > 
> > > > > claude.com
> > > > > 
> > > > > How Claude Code works in large codebases: Best practices and where to start | Claude
> > > > > 
> > > > > ![How Claude Code works in large codebases: Best practices and where to start | Claude](https://external-icn2-1.xx.fbcdn.net/emg1/v/t13/16980964102470988658?stp=dst-src&url=https%3A%2F%2Fcdn.prod.website-files.com%2F68a44d4040f98a4adf2207b6%2F6a060215e86f555c3638a2a1_og_how-claude-code-works-in-large-codebases-best-practices-and-where-to-start.jpg&utld=website-files.com&_nc_gid=xO7rbbq7OghR1ADLhm4Xmw&_nc_oc=AdryFH3OKQGJIviSO-cbvRWiEfnIILhvqgCx2elFfNxgi0QIJ-6zlJrEDSGXh84vr8M&ccb=13-1&oh=06_Q3_AAd4KkbaA4jIEF7U9ehr4HcCdMAF23ootTuZMAVpLV6RY&oe=6A2A830A&_nc_sid=1d65fc)
> > > > > 
> > > > > > **@choi.openai**
> > > > > > 
> > > > > > 2026-05-24
> > > > > > 
> > > > > > [https://www.threads.com/@choi.openai/post/DYtKAnoD0-S](https://www.threads.com/@choi.openai/post/DYtKAnoD0-S)