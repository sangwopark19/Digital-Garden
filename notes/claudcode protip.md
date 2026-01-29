## 컨텍스트 관리 전략 1: 단일 목적 대화 (ykdojo #14)
하나의 대화(터미널 세션)에서는 하나의 명확한 목표에만 집중하세요. 로그인 기능을 구현하다가 갑자기 “아, 그리고 대시보드 UI도 개선해줘”라고 하면 컨텍스트가 오염됩니다. 대신 새로운 터미널 탭을 열어 별도의 대화를 시작하세요.
## 컨텍스트 관리 전략 2: 선제적 압축 (ykdojo #8)
대화가 길어지면, 다음 에이전트를 위해 HANDOFF.md 파일을 작성하도록 지시하고 /clear 로 새롭게 시작하세요.
ykdojo의 실제 프롬프트:
**중요**

>“Put the rest of the plan in the system-prompt-extraction folder as HANDOFF.md. Explain what you have tried,
what worked, what didn’t work, so that the next agent with fresh context is able to just load that file and nothing
else to get started on this task and finish it up.”

Claude는 다음과 같은 내용을 포함한 HANDOFF.md를 생성합니다:
```markdown
# System Prompt Slimming - Handoff Document
## Goal
Reduce Claude Code's system prompt by ~45% (currently at 11%, need ~34% more).
## Current Progress
### What's Been Done
- Removed verbose examples from tool descriptions
- Shortened permission explanations
- Consolidated redundant instructions
### What Worked
- Regex-based pattern matching for finding repetitive text
- Testing each change individually before committing
### What Didn't Work
- Removing safety warnings (caused unexpected behavior)
- Over-aggressive minification (broke JSON parsing)
## Next Steps
1. Target the MCP server descriptions (estimated 2k tokens)
2. Simplify the hooks documentation
3. Test thoroughly after each change
```