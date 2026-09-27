# 🤖 Claude Code 101 — Developer Notes

> 📝 Personal notes from completing Anthropic's Claude Code 101 course.
> ✅ [Verify completion](https://verify.skilljar.com/c/sj39xz29o94s)

---

## 1️⃣ What is Claude Code?

Claude Code is an **agentic coding tool** 🛠️ — unlike Claude.ai (chat only), it has *direct* access to your filesystem, terminal, and codebase.

```
💬 Claude.ai    →  You copy-paste code back and forth
🤖 Claude Code  →  It reads, edits, and runs things itself
```

**Available everywhere you code:**

| Surface | Best for |
|---|---|
| 🖥️ Terminal | Bleeding-edge features first |
| 🔵 VS Code | Native IDE experience |
| 🟠 JetBrains | Native IDE experience |
| 🖱️ Desktop app | Background / async work |
| 🌐 Web (`claude.ai/code`) | Remote work on GitHub repos |

### 🔁 Why "agentic"?

```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│   Prompt    │ ───▶ │  LLM + Tools │ ───▶ │   Action    │
│  (your ask) │      │  (loop)      │      │ (file edit, │
└─────────────┘      └──────────────┘      │  cmd, etc.) │
       ▲                     │             └─────────────┘
       └─────────────────────┘
        observes result, loops again
```

**What it can actually do:**
- 🔍 Read & understand your codebase (explain a feature, trace a bug)
- ✏️ Edit files across the project (refactor + update every reference)
- ⚙️ Run terminal commands (tests, builds, installs) and react to output
- 🌐 Search the web for docs / latest APIs

### 🧠 Three things to always keep in mind

| Concept | What it means |
|---|---|
| 🧠 Context window | Claude's "working memory" — finite, used strategically |
| 🔐 Permissions | Asks before editing/running by default — you're in control |
| ⚠️ Imperfection | Can misread intent or over-engineer — stay in the loop |

---

## 2️⃣ Installation 📦

```
🖥️  macOS / Linux / WSL   →  curl installer   (brew install = no auto-update)
🪟  Windows (PowerShell)  →  Invoke-RestMethod
🪟  Windows (CMD)         →  curl
🪟  Windows (winget)      →  winget install   (no auto-update)
```

| Platform | Steps |
|---|---|
| 🔵 VS Code | Extensions → search "Claude Code" (✅ Anthropic verified) → Install → `Ctrl/Cmd+Shift+P` → "Open in New Tab" |
| 🟠 JetBrains | Install plugin from Marketplace → restart IDE |
| 🖱️ Desktop | Sign in → toggle "Code" mode at top |
| 🌐 Web | Go to `claude.ai/code` (GitHub repos only) |

> 📁 Running `claude` in a folder scopes access to that folder + subfolders.

---

## 3️⃣ Prompting & Modes 🎛️

```
⇧ Tab  ──cycles──▶  🟡 Approval Mode   →  asks before every edit/command
⇧ Tab  ──cycles──▶  🟢 Auto-Accept     →  edits auto-approved, commands still ask
⇧ Tab  ──cycles──▶  🔵 Plan Mode       →  read-only research → proposes a plan first
```

💡 **Plan Mode** = best for complex, multi-step changes — course-correct *before* any code is written.

---

## 4️⃣ Core Workflow 🔄

```
🔍 EXPLORE  ──▶  📋 PLAN  ──▶  💻 CODE  ──▶  ✅ COMMIT
   research      proposal    execution    review + ship
```

### 🔍📋 Explore & Plan
Use Plan Mode → Claude reads/researches before touching anything.

```
Prompt: "I need to add WebP conversion to our image upload pipeline.
Figure out where it should happen, what deps we need, and how
to approach it."
```

### 💻 Code
Approve the plan → Claude executes step by step.

- ✅ Define clear **success criteria** upfront
- ✅ Give it **tools** (e.g. Claude in Chrome, for testing UIs directly)
- ✅ Provide a **test suite** to validate against
- ✅ Repeated same bug? → Tell Claude to save the fix to `CLAUDE.md`

### ✅ Commit

```
👀 Fresh-eyes subagent review  ──▶  ✍️ Generate commit message  ──▶  🚀 Push
```

---

## 5️⃣ Context Management 🧠

```
/compact   →  🗜️  summarize session, keep working  (mid-feature)
/clear     →  🧹  wipe everything                   (starting something new)
/context   →  📊  see what's eating your context window
```
![Uploading image.png…]()

**💡 Tips to save context:**
- 🎯 Be specific — vague prompts cost *more* context (forces more exploring)
- 🔌 Disable unused MCP servers (they load tool defs into context by default)
- 🤖 Use subagents for lookups where you only need the *answer*

---

## 6️⃣ Git Workflow Features 🌿

| Feature | What it does |
|---|---|
| 👀 Subagent code review | Fresh, unbiased eyes before pushing — keep it read-only, commit config for the team |
| ⚡ `/commit-push-pr` skill | commit → push → PR, all in one step (can auto-post to Slack) |
| 🔗 `claude --from-pr <NUM>` | Resume a session linked to an existing PR |

---

## 7️⃣ `CLAUDE.md` — Persistent Memory 🧾

Auto-loaded every session. Think: **onboarding doc for Claude.**

```
# Project
Next.js 15, App Router, Tailwind, Drizzle ORM 🧩

# Commands 🖥️
- Dev:  pnpm dev
- Test: pnpm test
- Lint: pnpm lint

# Code Style 🎨
- 2-space indent
- Named exports
- API routes in app/api/
```

```
📁 Project-level CLAUDE.md   →  committed, shared with 👥 team
👤 User-level CLAUDE.md      →  personal, applies across ALL your projects
```

💡 Reference other docs inline: `@README.md`
💡 Start **without** one → notice repeated corrections → run `/init` (or build manually) → keeps it lean

---

## 8️⃣ Subagents 🧬

```
Main Agent 🤖
   │
   ├──▶ 🔬 Subagent A (own context window) ──▶ 📄 returns summary only
   ├──▶ 🔬 Subagent B (own context window) ──▶ 📄 returns summary only
   │
   ▼
🧠 clean main context
```

- Create via `/agents` → "Create new agent" (scope, purpose, tools, color 🎨)
- 💾 Can have persistent memory across sessions
- 🎁 Can preload skills (full skill content loads into *their* context)

---

## 9️⃣ MCP (Model Context Protocol) 🔌

```
Claude Code  🔌──▶  MCP Server  🔌──▶  External Tool (Linear, Context7, etc.)
```

| Type | Description |
|---|---|
| ☁️ HTTP | Remote, hosted by service provider |
| 💻 Stdio | Local process on your machine |
<img width="3840" height="2160" alt="image" src="https://github.com/user-attachments/assets/162ffca1-64f9-4305-9e15-444b526f85b5" />

**Scopes:**

```
🔒 Local    →  just you, this project
👤 User     →  you, all projects
📁 Project  →  .mcp.json, shared via version control 👥
```

- Manage: `/mcp` (check status, disable unused)
- ⚠️ Cost: unused MCP tool defs still sit in context
- 💡 A CLI (`gh`, `aws`) or a **Skill** is often more context-efficient

---

## 🔟 Hooks 🪝

**Deterministic** — unlike `CLAUDE.md` instructions, hooks *always* fire. No exceptions.

```
⚡ PreToolUse        →  before a tool runs    (can BLOCK it ⛔)
✅ PostToolUse       →  after a tool completes
⌨️  UserPromptSubmit  →  when you hit enter, before Claude processes it
🛑 Stop              →  when Claude finishes responding
🔔 Notification      →  when Claude sends a notification
```
<img width="3840" height="2160" alt="image" src="https://github.com/user-attachments/assets/ed8382c9-5ffb-46c2-bcf7-f4784bbe365d" />

**Common uses:**
- 🎨 Auto-format after edits (`PostToolUse`, matcher: `Edit|MultiEdit|Write`)
- ⛔ Block dangerous ops (`PreToolUse`, exit code `2` = block + feedback to Claude)
- 📋 Compliance logging, ✅ task-complete notifications

```
Exit code 0  →  ✅ proceed normally
Exit code 2  →  ⛔ block the action (stderr → fed back to Claude)
Other        →  ⚠️ non-blocking error, shown to you only
```

Configure via `/hooks` or `settings.json`. Project-level hooks (`.claude/settings.json`) can be committed so the whole team gets them — use `CLAUDE_PROJECT_DIR` to reference project scripts reliably.

> 💡 **Rule of thumb:** if something must happen every time, no exceptions → use a **hook**, not a prompt instruction.

---

## 🎯 Key Takeaways

- 🤖 Claude Code turns AI from "suggest code" into "**does** the work," with guardrails (permissions, plan mode) to keep you in control
- 🔄 **Explore → Plan → Code → Commit** is the biggest lever for quality output
- 🧠 **Context is a finite resource** — manage it via specific prompts, subagents, and pruning unused MCP servers/skills
- 🧾 **`CLAUDE.md`** + 🪝 **Hooks** = how you make Claude's behavior consistent and shareable across a team, not just a one-off habit
