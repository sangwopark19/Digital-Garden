# The Shorthand Guide to Everything Claude Code

**작성자:** cogsec (@affaanmustafa)
**날짜:** 오후 1:16 · 2026년 1월 17일
**원본 링크:** https://x.com/affaanmustafa/status/2012378465664745795

---

## 📊 게시물 통계

- 💬 답글: 116
- 🔄 재게시: 890
- ❤️ 마음에 들어요: 7,159
- 🔖 북마크: 21,679
- 👁️ 조회수: 261.2만

---

## 📝 게시물 내용

> Here's my complete setup after 10 months of daily use: skills, hooks, subagents, MCPs, plugins, and what actually works.

10개월간의 매일 사용 경험 후 완성된 설정을 공유합니다: skills, hooks, subagents, MCPs, plugins, 그리고 실제로 작동하는 것들

---

## 🏆 배경

Been an avid Claude Code user since the experimental rollout in Feb, and won the Anthropic x Forum Ventures hackathon with **Zenith** alongside @DRodriguezFX completely using Claude Code.

2월부터 Claude Code 베타 사용자였고, Anthropic x Forum Ventures 해커톤에서 @DRodriguezFX와 함께 **Zenith**로 우승했습니다 (완전히 Claude Code만 사용함).

---

## 🛠️ Skills and Commands

Skills operate like rules, constricted to certain scopes and workflows. They're shorthand to prompts when you need to execute a particular workflow.

### 사용 예

- 긴 코딩 세션 후 죽은 코드와 느슨한 .md 파일을 정리하려면: `/refactor-clean`
- 테스트 필요? `/tdd`, `/e2e`, `/test-coverage`
- Skills와 commands는 한 번의 프롬프트에서 체인될 수 있음

### 저장 위치

```
~/.claude/skills/
├── pmx-guidelines.md           # Project-specific patterns
├── coding-standards.md         # Language best practices
├── tdd-workflow/               # Multi-file skill with README.md
└── security-review/            # Checklist-based skill
```

### Skills vs Commands

| 타입 | 설명 | 위치 |
|------|------|------|
| **Skills** | broader workflow definitions | `~/.claude/skills` |
| **Commands** | quick executable prompts | `~/.claude/commands` |

---

## 🎣 Hooks

Hooks are trigger-based automations that fire on specific events. Unlike skills, they're constricted to tool calls and lifecycle events.

### Hook 타입

1. **PreToolUse** - Before a tool executes (validation, reminders)
2. **PostToolUse** - After a tool finishes (formatting, feedback loops)
3. **UserPromptSubmit** - When you send a message
4. **Stop** - When Claude finishes responding
5. **PreCompact** - Before context compaction
6. **Notification** - Permission requests

### 예시: tmux 리마인더

```json
{
  "PreToolUse": [
    {
      "matcher": "tool == \"Bash\" && tool_input.command matches \"(npm|pnpm|yarn|cargo|pytest)\"",
      "hooks": [
        {
          "type": "command",
          "command": "if [ -z \"$TMUX\" ]; then echo '[Hook] Consider tmux for session persistence' >&2; fi"
        }
      ]
    }
  ]
}
```

**Pro tip:** Use the `hookify` plugin to create hooks conversationally. Run `/hookify` and describe what you want.

---

## 🤖 Subagents

Subagents are processes your orchestrator (main Claude) can delegate tasks to with limited scopes. They can run in background or foreground, freeing up context for the main agent.

### 저장 위치

```
~/.claude/agents/
├── planner.md              # Feature implementation planning
├── architect.md            # System design decisions
├── tdd-guide.md            # Test-driven development
├── code-reviewer.md        # Quality/security review
├── security-reviewer.md    # Vulnerability analysis
├── build-error-resolver.md
├── e2e-runner.md
└── refactor-cleaner.md
```

Configure allowed tools, MCPs, and permissions per subagent for proper scoping.

---

## 📚 Rules and Memory

Your `.rules` folder holds `.md` files with best practices Claude should ALWAYS follow.

### 두 가지 접근 방식

1. **Single CLAUDE.md** - Everything in one file (user or project level)
2. **Rules folder** - Modular `.md` files grouped by concern

### 저장 위치

```
~/.claude/rules/
├── security.md        # No hardcoded secrets, validate inputs
├── coding-style.md    # Immutability, file organization
├── testing.md         # TDD workflow, 80% coverage
├── git-workflow.md    # Commit format, PR process
├── agents.md          # When to delegate to subagents
└── performance.md     # Model selection, context management
```

### 예시 규칙

- No emojis in codebase
- Refrain from purple hues in frontend
- Always test code before deployment
- Prioritize modular code over mega-files
- Never commit console.logs

---

## 🔌 MCPs (Model Context Protocol)

MCPs connect Claude to external services directly. Not a replacement for APIs - it's a prompt-driven wrapper around them, allowing more flexibility in navigating information.

**Example:** Supabase MCP lets Claude pull specific data, run SQL directly upstream without copy-paste. Same for databases, deployment platforms, etc.

**Chrome in Claude:** is a built-in plugin MCP that lets Claude autonomously control your browser - clicking around to see how things work.

### ⚠️ CRITICAL: Context Window Management

Be picky with MCPs. Keep all MCPs in user config but **disable everything unused**.

Navigate to `/plugins` and scroll down or run `/mcp`.

**Rule of thumb:** Have 20-30 MCPs in config, but keep under 10 enabled / under 80 tools active.

Your 200k context window before compacting might only be 70k with too many tools enabled. Performance degrades significantly.

---

## 🧩 Plugins

Plugins package tools for easy installation instead of tedious manual setup. A plugin can be a skill + MCP combined, or hooks/tools bundled together.

### 플러그인 설치

```bash
# Add a marketplace
claude plugin marketplace add https://github.com/mixedbread-ai/mgrep

# Open Claude, run /plugins, find new marketplace, install from there
```

### LSP Plugins

Particularly useful if you run Claude Code outside editors frequently. Language Server Protocol gives Claude real-time type checking, go-to-definition, and intelligent completions without needing an IDE open.

```bash
# Enabled plugins example
typescript-lsp@claude-plugins-official     # TypeScript intelligence
pyright-lsp@claude-plugins-official        # Python type checking
hookify@claude-plugins-official            # Create hooks conversationally
mgrep@Mixedbread-Grep                      # Better search than ripgrep
```

**Same warning as MCPs** - watch your context window.

---

## 💡 Tips and Tricks

### 키보드 단축키

| 단축키 | 기능 |
|--------|------|
| `Ctrl+U` | Delete entire line (faster than backspace spam) |
| `!` | Quick bash command prefix |
| `@` | Search for files |
| `/` | Initiate slash commands |
| `Shift+Enter` | Multi-line input |
| `Tab` | Toggle thinking display |
| `Esc Esc` | Interrupt Claude / restore code |

### Parallel Workflows

`/fork` - Fork conversations to do non-overlapping tasks in parallel instead of spamming queued messages

### Git Worktrees

For overlapping parallel Claudes without conflicts. Each worktree is an independent checkout.

```bash
git worktree add ../feature-branch feature-branch
# Now run separate Claude instances in each worktree
```

### tmux for Long-Running Commands

Stream and watch logs/bash processes Claude runs.

```bash
tmux new -s dev
# Claude runs commands here, you can detach and reattach
tmux attach -t dev
```

### mgrep > grep

`mgrep` is a significant improvement from ripgrep/grep. Install via plugin marketplace, then use the `/mgrep` skill. Works with both local search and web search.

```bash
mgrep "function handleSubmit"              # Local search
mgrep --web "Next.js 15 app router changes"  # Web search
```

### 기타 유용한 명령어

| 명령어 | 기능 |
|--------|------|
| `/rewind` | Go back to a previous state |
| `/statusline` | Customize with branch, context %, todos |
| `/checkpoints` | File-level undo points |
| `/compact` | Manually trigger context compaction |

### GitHub Actions CI/CD

Set up code review on your PRs with GitHub Actions. Claude can review PRs automatically when configured.

### Sandboxing

Use sandbox mode for risky operations - Claude runs in restricted environment without affecting your actual system.

---

## 🖥️ On Editors

### Zed (My Preference)

I use **Zed** - a Rust-based editor that's lightweight, fast, and highly customizable.

#### Why Zed works well with Claude Code:

- **Agent Panel Integration** - Zed's Claude integration lets you track file changes in real-time as Claude edits. Jump between files Claude references without leaving the editor
- **Performance** - Written in Rust, opens instantly and handles large codebases without lag
- **CMD+Shift+R Command Palette** - Quick access to all your custom slash commands, debuggers, and tools in a searchable UI
- **Minimal Resource Usage** - Won't compete with Claude for system resources during heavy operations
- **Vim Mode** - Full vim keybindings if that's your thing

#### Zed 사용 팁

- **Split your screen** - Terminal with Claude Code on one side, editor on the other
- **Ctrl + G** - quickly open the file Claude is currently working on in Zed
- **Auto-save** - Enable autosave so Claude's file reads are always current
- **Git integration** - Use editor's git features to review Claude's changes before committing
- **File watchers** - Most editors auto-reload changed files, verify this is enabled

### VSCode / Cursor

This is also a viable choice and works well with Claude Code. You can use it in either terminal format, with automatic sync with your editor using `\ide` enabling LSP functionality, or you can opt for the extension which is more integrated with the Editor and has a matching UI.

See docs: https://code.claude.com/docs/en/vs-code

---

## 🎯 My Setup

### Plugins (usually only 4-5 enabled at a time)

```markdown
- ralph-wiggum@claude-code-plugins     # Loop automation
- frontend-design@claude-code-plugins  # UI/UX patterns
- commit-commands@claude-code-plugins  # Git workflow
- security-guidance@claude-code-plugins # Security checks
- pr-review-toolkit@claude-code-plugins # PR automation
- typescript-lsp@claude-plugins-official # TS intelligence
- hookify@claude-plugins-official      # Hook creation
- code-simplifier@claude-plugins-official
- feature-dev@claude-code-plugins
- explanatory-output-style@claude-code-plugins
- code-review@claude-code-plugins
- context7@claude-plugins-official     # Live documentation
- pyright-lsp@claude-plugins-official  # Python types
- mgrep@Mixedbread-Grep               # Better search
```

### MCP Servers Configured (User Level)

```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"]
  },
  "firecrawl": {
    "command": "npx",
    "args": ["-y", "firecrawl-mcp"]
  },
  "supabase": {
    "command": "npx",
    "args": ["-y", "@supabase/mcp-server-supabase@latest", "--project-ref=YOUR_REF"]
  },
  "memory": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-memory"]
  },
  "sequential-thinking": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
  },
  "vercel": {
    "type": "http",
    "url": "https://mcp.vercel.com"
  },
  "railway": {
    "command": "npx",
    "args": ["-y", "@railway/mcp-server"]
  },
  "cloudflare-docs": {
    "type": "http",
    "url": "https://docs.mcp.cloudflare.com/mcp"
  },
  "cloudflare-workers-bindings": {
    "type": "http",
    "url": "https://bindings.mcp.cloudflare.com/mcp"
  },
  "cloudflare-workers-builds": {
    "type": "http",
    "url": "https://builds.mcp.cloudflare.com/mcp"
  },
  "cloudflare-observability": {
    "type": "http",
    "url": "https://observability.mcp.cloudflare.com/mcp"
  },
  "clickhouse": {
    "type": "http",
    "url": "https://mcp.clickhouse.cloud/mcp"
  },
  "AbletonMCP": {
    "command": "uvx",
    "args": ["ableton-mcp"]
  },
  "magic": {
    "command": "npx",
    "args": ["-y", "@magicuidesign/mcp@latest"]
  }
}
```

### Disabled per Project (context window management)

```markdown
# In ~/.claude.json under projects.[path].disabledMcpServers

disabledMcpServers: [
  "playwright",
  "cloudflare-workers-builds",
  "cloudflare-workers-bindings",
  "cloudflare-observability",
  "cloudflare-docs",
  "clickhouse",
  "AbletonMCP",
  "context7",
  "magic"
]
```

**This is the key** - I have 14 MCPs configured but only ~ 5-6 enabled per project. Keeps context window healthy.

### Key Hooks

```json
{
  "PreToolUse": [
    {
      "matcher": "npm|pnpm|yarn|cargo|pytest",
      "hooks": ["tmux reminder"]
    },
    {
      "matcher": "Write && .md file",
      "hooks": ["block unless README/CLAUDE"]
    },
    {
      "matcher": "git push",
      "hooks": ["open editor for review"]
    }
  ],
  "PostToolUse": [
    {
      "matcher": "Edit && .ts/.tsx/.js/.jsx",
      "hooks": ["prettier --write"]
    },
    {
      "matcher": "Edit && .ts/.tsx",
      "hooks": ["tsc --noEmit"]
    },
    {
      "matcher": "Edit",
      "hooks": ["grep console.log warning"]
    }
  ],
  "Stop": [
    {
      "matcher": "*",
      "hooks": ["check modified files for console.log"]
    }
  ]
}
```

### Custom Status Line

Shows user, directory, git branch with dirty indicator, context remaining %, model, time, and todo count.

### Rules Structure

```
~/.claude/rules/
├── security.md           # Mandatory security checks
├── coding-style.md       # Immutability, file size limits
├── testing.md            # TDD, 80% coverage
├── git-workflow.md       # Conventional commits
├── agents.md             # Subagent delegation rules
├── patterns.md           # API response formats
├── performance.md        # Model selection (Haiku vs Sonnet vs Opus)
└── hooks.md              # Hook documentation
```

### Subagents

```
~/.claude/agents/
├── planner.md             # Break down features
├── architect.md           # System design
├── tdd-guide.md           # Write tests first
├── code-reviewer.md       # Quality review
├── security-reviewer.md   # Vulnerability scan
├── build-error-resolver.md
├── e2e-runner.md          # Playwright tests
├── refactor-cleaner.md    # Dead code removal
└── doc-updater.md         # Keep docs synced
```

---

## 🎓 Key Takeaways

- ✅ **Don't overcomplicate** - treat configuration like fine-tuning, not architecture
- ✅ **Context window is precious** - disable unused MCPs and plugins
- ✅ **Parallel execution** - fork conversations, use git worktrees
- ✅ **Automate the repetitive** - hooks for formatting, linting, reminders
- ✅ **Scope your subagents** - limited tools = focused execution

---

## 📖 References

- [Plugins Reference](https://code.claude.com/docs/en/plugins-reference)
- [Hooks Documentation](https://code.claude.com/docs/en/hooks)
- [Checkpointing](https://code.claude.com/docs/en/checkpointing)
- [Interactive Mode](https://code.claude.com/docs/en/interactive-mode)
- [Memory System](https://code.claude.com/docs/en/memory)
- [Subagents](https://code.claude.com/docs/en/sub-agents)
- [MCP Overview](https://code.claude.com/docs/en/mcp-overview)

---

**Note:** This is a subset of detail. More posts on specifics may come if people are interested.

---

*이 노트는 X의 게시물을 마크다운 형식으로 변환하여 저장되었습니다.*
