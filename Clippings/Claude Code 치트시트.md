---
title: "Claude Code 치트시트"
source: "https://news.hada.io/topic?id=27806"
author:
  - "[[neo]]"
published: 2026-03-24
created: 2026-03-26
description: "클로드 코드 최신 버전의 주요 명령어, 단축키, 설정, 환경 변수, MCP 서버 및 에이전트 구성을 정리한 개발자용 요약 문서새 버전에는 헤드리스 모드(--bare), MCP로 디스코드/텔레그램 메시지 전송(--channels), 스킬/슬래시 명령를 위한 프론트매터(effort), fork 가 /branch 로 변경, SendMessage 자동 재개 기능이"
tags:
  - "clippings"
---
▲

(cc.storyfox.cz)

39P by [GN⁺](https://news.hada.io/user?id=neo) 2일전 | ★ favorite | [댓글 1개](https://news.hada.io/topic?id=27806)

- **클로드 코드 최신 버전** 의 주요 명령어, 단축키, 설정, 환경 변수, MCP 서버 및 에이전트 구성을 정리한 **개발자용 요약 문서**
- 새 버전에는 헤드리스 모드(`--bare`), **MCP로 디스코드/텔레그램 메시지 전송** (`--channels`), **스킬/슬래시 명령** 를 위한 프론트매터(`effort`), `fork` 가 `/branch` 로 변경, **`SendMessage` 자동 재개** 기능이 추가됨
- **키보드 단축키**, **MCP 서버**, **슬래시 명령어**, **스킬·에이전트 관리**, **헤드리스 실행 및 원격 제어** 등 대부분의 명령어를 보기 좋게 정리
- 윈도우/맥용 별도로 보기 스위치 지원

---

## 키보드 단축키

- ### 일반 제어
	- `Ctrl C` 입력/생성 취소, `Ctrl D` 세션 종료, `Ctrl L` 화면 지우기, `Ctrl O` 상세 출력 전환, `Ctrl R` 히스토리 검색, `Ctrl G` 프롬프트 편집기 열기
		- `Ctrl B` 백그라운드 실행, `Ctrl T` 작업 목록 전환, `Ctrl V` 이미지 붙여넣기, `Ctrl F` 백그라운드 에이전트 종료(2회 필요), `Esc` 되돌리기
- ### 모드 전환
	- `Shift Tab` 권한 모드 순환, `Alt P` 모델 전환, `Alt T` 사고(thinking) 모드 전환
- ### 입력 제어
	- `Enter` 빠른 줄바꿈, `Ctrl J` 제어 시퀀스 줄바꿈
- ### 접두사
	- `/` 슬래시 명령, `!` 직접 bash 실행, `@` 파일 언급 및 자동완성
- ### 세션 선택기
	- 방향키로 탐색 및 확장/축소, `P` 미리보기, `R` 이름 변경, `/` 검색, `A` 전체 프로젝트, `B` 현재 브랜치

## MCP 서버 관리

- ### 서버 추가
	- `--transport http` 원격 HTTP(권장), `--transport stdio` 로컬 프로세스, `--transport sse` 원격 SSE
- ### 스코프
	- 로컬(`~/.claude.json`), 프로젝트(`project.mcp.json`), 사용자(`~/.claude.json`)
- ### 관리 명령
	- `/mcp` 인터랙티브 UI, `claude mcp list` 전체 서버 목록, `claude mcp serve` CC를 MCP 서버로 실행
- ### Elicitation Servers
	- 작업 중 입력 요청 기능(신규)

## 슬래시 명령어

- ### 세션 관련
	- `/clear`, `/compact`, `/resume`, `/rename`, `/branch`, `/cost`, `/context`, `/diff`, `/copy`, `/export`
- ### 설정 관련
	- `/config`, `/model`, `/fast`, `/vim`, `/theme`, `/permissions`, `/effort`, `/color`
- ### 도구 관련
	- `/init`, `/memory`, `/mcp`, `/hooks`, `/skills`, `/agents`, `/chrome`, `/reload-plugins`
- ### 특수 명령
	- `/btw`, `/plan`, `/loop`, `/voice`, `/doctor`, `/rc`, `/pr-comments`, `/stats`, `/insights`, `/desktop`, `/remote-control`, `/stickers`

## 메모리 및 파일 구조

- ### CLAUDE.md 위치
	- 프로젝트(`./CLAUDE.md`), 개인(`~/.claude/CLAUDE.md`), 조직(`/etc/claude-code/Managed`)
- ### 규칙 및 임포트
	- `.claude/rules/*.md`, `~/.claude/rules/*.md`, `@path/to/file` 임포트 가능
- ### 자동 메모리
	- `~/.claude/projects//memory/` 내 `MEMORY.md` 및 주제별 파일 자동 로드

## 워크플로와 팁

- ### Plan Mode
	- `Shift Tab` 으로 일반→자동→계획 모드 전환, `--permission-mode plan` 으로 시작 가능
- ### Thinking & Effort
	- `Alt T` 사고 모드 전환, `"ultrathink"` 최대 노력 모드, `/effort` 로 수준 설정(`low`, `med`, `high`)
- ### Git Worktrees
	- `--worktree` 로 기능별 분리 브랜치 생성, `sparsePaths` 로 필요한 디렉터리만 체크아웃
- ### Voice Mode
	- `/voice` 로 음성 입력 활성화, 스페이스바로 녹음 및 전송, 20개 언어 지원
- ### Context 관리
	- `/context`, `/compact` 로 컨텍스트 최적화, 최대 1M 컨텍스트 지원, `CLAUDE.md` 는 압축 후에도 유지
- ### 세션 단축 명령
	- `claude -c` 마지막 대화 이어가기, `claude -r "name"` 이름으로 재개, `/btw` 로 별도 질문

## SDK 및 헤드리스 모드

- ### 비대화형 실행
	- `claude -p "query"`, `--output-format json`, `--max-budget-usd` 비용 제한, 파이프 입력 지원
- ### 스케줄링 및 원격
	- `/loop` 주기적 작업, `/rc` 원격 제어, `--remote` 로 웹 세션 연결

## 설정 및 환경

- ### 설정 파일
	- 사용자(`~/.claude/settings.json`), 프로젝트(`.claude/settings.json`), 로컬(`.claude/settings.local.json`)
		- OAuth, MCP, 상태(`~/.claude.json`), 프로젝트 MCP 서버(`.mcp.json`)
- ### 핵심 설정 항목
	- `modelOverrides`, `autoMemoryDirectory`, `worktree.sparsePaths`
- ### 환경 변수
	- `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL`, `CLAUDE_CODE_EFFORT_LEVEL`, `MAX_THINKING_TOKENS`, `ANTHROPIC_CUSTOM_MODEL_OPTION`, `CLAUDE_CODE_PLUGIN_SEED_DIR`

## 스킬 및 에이전트

- ### 내장 스킬
	- `/simplify`, `/batch`, `/debug`, `/loop`, `/claude-api`
- ### 커스텀 스킬 위치
	- 프로젝트(`.claude/skills//`), 개인(`~/.claude/skills//`)
- ### 스킬 프론트매터
	- `description`, `allowed-tools`, `model`, `effort`, `context`, `$ARGUMENTS`, `${CLAUDE_SKILL_DIR}`, `!cmd`
- ### 내장 에이전트
	- `Explore`, `Plan`, `General`, `Bash`
- ### 에이전트 프론트매터
	- `permissionMode`, `isolation`, `memory`, `background`, `maxTurns`, `SendMessage` (신규 자동 재개)

## CLI 및 플래그

- ### 핵심 명령
	- `claude`, `claude "q"`, `claude -p "q"`, `claude -c`, `claude -r`, `claude update`
- ### 주요 플래그
	- `--model`, `-w`, `-n`, `--add-dir`, `--agent`, `--allowedTools`, `--output-format`, `--json-schema`, `--max-turns`, `--max-budget-usd`, `--console`, `--verbose`, `--bare`, `--channels`, `--remote`, `--chrome`
- ### 권한 모드
	- `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions`
- ### 핵심 환경 변수
	- `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL`, `CLAUDE_CODE_EFFORT_LEVEL`, `MAX_THINKING_TOKENS`, `CLAUDE_CODE_MAX_OUTPUT_TOKENS`, `CLAUDE_CODE_DISABLE_CRON`

▲

###### Hacker News 의견들

- 나는 매일 **Claude Code** 를 사용하지만 명령어를 자주 잊어서, Claude에게 공식 문서와 GitHub에서 모든 기능을 조사하게 하고, 단축키·슬래시 명령어·워크플로·스킬 시스템·메모리/CLAUDE.md·MCP 설정·CLI 플래그·설정 파일을 한눈에 볼 수 있는 **A4 가로 HTML 치트시트** 를 만들게 했음  
	자동으로 Mac/Windows 단축키를 감지하고, 최신 버전과 변경 로그를 표시함. 매일 크론 잡이 변경 사항을 확인해 자동 업데이트하며 새 기능에는 “NEW” 배지를 붙임  
	가볍고 무료이며 회원가입이 필요 없음. [cc.storyfox.cz](https://cc.storyfox.cz/) 에서 Ctrl+P로 인쇄 가능하고 모바일에서도 작동함
	- “Ctrl+P로 인쇄 가능, 모바일에서도 작동”이라는 문구가 재밌음. 내 폰에는 Ctrl 키가 없고, Mac에서는 아마 **Cmd+P** 일 것 같음
		- 이 시트는 어떤 **Claude Code 버전** 기준인지 궁금함. 내 버전에는 `/cost` 명령이 없음
		- `^` 기호는 Control 키를 의미하고 `⌘` 는 아님
		- 혹시 **소스 코드 공개** 계획이 있는지 궁금함
		- 멋진 작업임. 고마움
- 나는 최근 CC 터미널에서 **VS Code 확장** 으로 전환했는데 훨씬 마음에 듦
	- 나도 같음. UI에서 작업하고, 리포지토리 파일을 탐색·리뷰·편집하기가 훨씬 쉬워짐
- “MCP” 섹션에서 “Local” 앞의 “~”는 잘못된 표기임. 프로젝트별 설정은 단순히 `.claude.json` 이어야 함
- “CMD + V로 이미지 붙여넣기”는 잘못된 정보임. Mac에서도 Windows처럼 **CTRL + V** 를 사용함. CMD + V는 텍스트 붙여넣기용임
	- **Warp Terminal** 에서는 Mac에서도 CMD + V로 이미지를 붙여넣을 수 있음
		- 다른 명령어도 비슷함. 예를 들어 외부 편집기를 여는 건 Mac에서도 CMD+G가 아니라 **CTRL+G** 임
		- Linux에서는 **CTRL + SHIFT + V** 를 쓰는 걸로 알고 있음. CTRL + V는 다른 키 조합으로 인식됨
- 환경 변수는 실제로 훨씬 많음. 내가 좋아하는 건 `IS_DEMO=1` 로, 불필요한 환영 배너를 제거해줌
	- 관련 문서는 [code.claude.com/docs/en/env-vars](https://code.claude.com/docs/en/env-vars) 에 있음
- ‘project rules’라는 개념이 실제로 존재하는지 궁금함  
	`.claude/rules/` 와 `~/.claude/rules/` 디렉터리가 있는데, 이게 단순히 다른 프롬프트에서 불러올 파일을 정리하는 용도인지 알고 싶음
- 이런 **기능 요약 시트** 를 만들어줘서 고마움. 새 기능이 자주 추가되는데, 한눈에 볼 수 있어 문서를 뒤질 필요가 줄어듦
- **Claude Code** 가 CLI 측면에서 Codex보다 훨씬 앞서 있다는 게 놀라움
	- 나는 Claude Code로 **자기 복제형 에이전트** 를 만들어봤음. 메인 브랜치에서 5개의 git worktree를 분기해 각각 독립적으로 작업하게 하고, 60초마다 성능을 분석해 더 나은 쪽으로 자기 자신을 개선함.  
		43회 반복 후에는 어떤 웹사이트든 다양한 프로토콜(WebSocket, GraphQL, gRPC-Web 등)을 **타입드 JSON API** 로 변환하는 데 10~30분밖에 걸리지 않음.  
		다음엔 4년치 주식·옵션 거래 데이터 263GB를 학습시켜 **거래 전략** 을 찾게 할 예정임. Claude Code가 AGI에 가장 먼저 도달할 것 같음
		- 하지만 너무 **느림**. 입력이 자주 씹히고, TUI라 빠를 줄 알았는데 그렇지 않음
		- 그런데 OpenAI가 인수한 사람들은 여전히 Codex가 “미래”라고 말함
		- 실제로 **성능 면에서는 Codex** 가 Claude Code보다 낫다고 느낌
- 페이지의 변경 로그 링크를 보고, 변경 이력을 시각화해봤음. ChatGPT에게 CHANGELOG.md의 일별 추가 항목 수를 그래프로 그리게 했고, 대략적으로 맞는 것 같음  
	[imgur.com/a/tky9Pkz](https://imgur.com/a/tky9Pkz)
- “Undo (입력 취소)”는 **Ctrl + \_ (Ctrl + 언더스코어)** 로 작동함. CC 외부의 라인 에디터에서도 동일하게 적용됨

답변달기