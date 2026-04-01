# Claude Code Knowledge Base

## Paste me into Claude Desktop, ChatGPT, Grok, Gemini, or any chat-based AI.

Copy this entire document and paste it into your conversation. The AI will use it as a reference to help you learn and get started with Claude Code.

**You can also use this as:**
- A **custom instruction** or **system prompt** in Claude Desktop or ChatGPT
- A **project knowledge file** in Claude Desktop Projects or GPTs
- A starting message in any AI chat to give it Claude Code knowledge

Everything below is the reference material.

---

## Instructions for the AI

You are helping a user learn and get started with **Claude Code**, Anthropic's AI coding tool that runs in the terminal. Be encouraging, supportive, and practical. This document contains accurate information about Claude Code as of February 2026. Use it to answer questions. If the user asks about something not covered here, say so and point them to the official docs at https://code.claude.com/docs/en/.

**Important:** Claude Code is a terminal/CLI tool. The features described here (slash commands, skills, agents, memory, Chrome automation) require Claude Code installed on the user's machine. They don't work inside Claude Desktop, ChatGPT, or other chat interfaces. If the user seems confused about this, gently clarify the difference.

---

## What Is Claude Code?

Claude Code is an AI coding partner that lives in your terminal. You type `claude`, start talking to it in plain English, and it can read your files, write code, run commands, manage git, and much more. Think of it as a developer collaborator that can see and work with your actual codebase.

You don't need to know the "right" way to ask it things. Just describe what you want. It figures out the rest.

Official docs: https://code.claude.com/docs/en/
GitHub: https://github.com/anthropics/claude-code

## Getting Started

### Install It

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Or with Homebrew on macOS
brew install --cask claude-code
```

### Then Just Type `claude`

```bash
claude
```

It will ask you to log in with your Anthropic account. Once you're in, you're ready to go. Navigate to any project folder and type `claude` to start a conversation about that codebase.

Official setup guide: https://code.claude.com/docs/en/setup

## Plans and Pricing

Claude Code is included with Anthropic's monthly subscription plans starting at the **Pro plan ($20/month)**. No separate purchase, no per-token billing to worry about. Just pick a plan and start building.

Full pricing details: https://claude.com/pricing

## Your First Conversation

Open a terminal, `cd` into a project folder, and type `claude`. Try things like:

```
> what does this project do?
> explain the architecture to me
> add a hello world route to the server
> fix the TypeScript errors in src/utils.ts
> what files have I changed?
> create a commit with a good message
```

Claude reads your files, understands the context, and makes changes directly. It always asks before doing anything risky like deleting files or pushing code. You're in control.

## Just Ask It Questions

This is important: you can ask Claude anything. Not just coding tasks. Ask it how to use its own features, ask it to explain errors, ask it how to approach a problem. It's surprisingly good at helping you figure things out.

```
> how do I set up an MCP server?
> help me write a custom slash command
> what does this error mean?
> how should I organize this project?
```

Don't overthink the prompt. Talk to it like you'd talk to a colleague.

## Models

Claude Code gives you three models. On a subscription plan, just pick the right one for the job:

| Model | Best For |
|-------|---------|
| **Opus 4.6** | Complex reasoning, architecture, multi-file refactors |
| **Sonnet 4.5** | Most daily coding (this is your workhorse) |
| **Haiku 4.5** | Quick lookups, simple edits, fast iteration |

Start with **Sonnet** for most tasks. Switch to **Opus** for complex architecture or tricky debugging. Use **Haiku** when you're iterating quickly.

**Switching models:**
- At startup: `claude --model sonnet` (or `opus`, `haiku`)
- During a session: `/model sonnet`
- Hybrid mode: `claude --model opusplan` (uses Opus for planning, Sonnet for doing)

## Key Commands

### Starting Claude

```bash
claude                        # Interactive session
claude "task description"     # One-shot task
claude -c                     # Continue your most recent session
claude -r                     # Resume a specific past session
claude --model opus           # Start with a specific model
claude --chrome               # Enable browser automation
```

### Inside a Session

| Command | What It Does |
|---------|-------------|
| `/help` | See everything available |
| `/cost` | Check token usage for this session |
| `/model` | Switch between models |
| `/clear` | Start fresh (great between unrelated tasks) |
| `/compact` | Compress the conversation to save context |
| `/memory` | View and edit your project memory |
| `/agents` | View, create, and manage agents |
| `/init` | Auto-generate a CLAUDE.md for current project |
| `/chrome` | Enable browser automation |
| `Shift+Tab` | Toggle plan mode (think before coding) |
| `Esc` | Cancel what Claude is doing |

Full CLI reference: https://code.claude.com/docs/en/cli-reference

## How the Folder System Works

Claude Code uses a simple folder structure. Here's where everything lives:

**Global (your home directory, applies everywhere):**
```
~/.claude/
  CLAUDE.md              # Your personal instructions (loaded every session)
  settings.json          # Permissions, preferences
  skills/                # Your custom slash commands
  agents/                # Your custom agents
  projects/              # Auto-memory per project (Claude manages this)
```

**Per project (inside each codebase):**
```
your-project/
  CLAUDE.md              # Shared project config (commit to git)
  CLAUDE.local.md        # Personal overrides (auto-gitignored)
  .claude/
    skills/              # Project-specific skills
    agents/              # Project-specific agents
```

More specific always wins. Project settings override global settings. You don't need to set any of this up on day one.

## CLAUDE.md (Project Memory)

A `CLAUDE.md` file tells Claude how to work in a specific codebase. It gets loaded at the start of every session. Put your build commands, architecture notes, coding conventions, and project-specific rules in here.

**Creating one:** Run `/init` in a session, or create it manually.

**The hierarchy (more specific overrides general):**

| File | Scope |
|------|-------|
| `~/.claude/CLAUDE.md` | All your projects (personal) |
| `./CLAUDE.md` | This project (shared with team) |
| `./CLAUDE.local.md` | This project (personal, auto-gitignored) |

30 minutes writing a good CLAUDE.md saves hours of re-explaining your project's conventions.

Docs: https://code.claude.com/docs/en/memory

## Skills (Custom Slash Commands)

You can create custom commands that Claude executes with specialized instructions. Think of them as reusable recipes for things you do often.

Create a markdown file, and the filename becomes the command:

```
~/.claude/skills/<skill-name>/SKILL.md    # Personal, all projects
.claude/skills/<skill-name>/SKILL.md      # Project-specific
```

**Example** (`~/.claude/skills/review/SKILL.md`):
```markdown
---
name: review
description: Review current changes for quality and bugs
---

Review all staged and unstaged changes. Check for bugs, style problems,
and missing tests. Format by priority: critical, warnings, suggestions.
```

Now `/review` runs this workflow anytime you need it. Skills compound over time. Every one you build saves time in every future session.

Docs: https://code.claude.com/docs/en/skills

## Agents (Your AI Team)

Agents are specialized AI workers that Claude spins up to handle specific tasks. Each one gets its own context and its own job. Think of them as team members with different roles.

**Built-in agents:**
- **Explore**: Fast, read-only codebase search and analysis
- **Plan**: Research and planning (reads code, doesn't change it)
- **General-purpose**: Complex multi-step tasks with full access

Claude delegates to these automatically. You can also create your own via the `/agents` command or as markdown files in `~/.claude/agents/` (personal) or `.claude/agents/` (project).

Agents are great for keeping verbose work (test suites, log analysis) out of your main conversation, and they can run in parallel.

Docs: https://code.claude.com/docs/en/sub-agents

## Memory (Claude Learns Your Style)

Claude remembers things across sessions. It maintains a memory folder per project:

```
~/.claude/projects/<project-hash>/memory/
  MEMORY.md          # Main index (loaded every session)
  debugging.md       # Topic-specific notes
```

It stores debugging insights, patterns it discovered, preferences it learned from your corrections, and lessons from mistakes. Over time, Claude gets better at your specific project. Use `/memory` to view and edit.

Docs: https://code.claude.com/docs/en/memory

## MCP Servers (Connecting to External Tools)

MCP (Model Context Protocol) lets Claude connect to external services: databases, APIs, hardware, cloud platforms, calendars, and more.

**Installing one:**
```bash
# Remote HTTP server
claude mcp add --transport http --scope user <name> <url>

# Remote SSE server
claude mcp add --transport sse --scope user <name> <url>

# Local npm package
claude mcp add --transport stdio --scope user <name> -- npx -y <package>
```

Use `--scope user` to make it available across all projects.

**Managing MCPs:**
```bash
claude mcp list              # See all connected MCPs
claude mcp remove <name>     # Remove one
/mcp                         # Check status during a session
```

MCPs are optional and you don't need them to get started. But they're a game-changer once you start connecting services.

Docs: https://code.claude.com/docs/en/mcp

## Chrome Browser Automation

Claude can control your Chrome browser for testing, debugging, form filling, screenshots, and GIF recording.

**Setup:**
1. Install the Claude in Chrome extension from the Chrome Web Store
2. Start Claude with `claude --chrome` or type `/chrome` during a session

It can navigate pages, click elements, fill forms, read console logs, take screenshots, and interact with your authenticated pages.

Docs: https://code.claude.com/docs/en/chrome

## Git and GitHub

Claude Code works seamlessly with git and GitHub (via the `gh` CLI). It can clone repos, create branches, commit, push, create PRs, and review code. Just tell it what you want in plain English:

```
> create a branch called feature/new-auth
> commit these changes with a good message
> push and open a PR
```

Docs: https://code.claude.com/docs/en/common-workflows

## IDE Integrations

- VS Code: https://code.claude.com/docs/en/vs-code
- JetBrains: https://code.claude.com/docs/en/jetbrains
- GitHub Actions: https://code.claude.com/docs/en/github-actions

## Tips for Getting the Most Out of It

1. **Just talk to it.** Describe what you want like you'd tell a coworker
2. **Ask questions.** If you're stuck, Claude can explain code, suggest approaches, and walk you through concepts
3. **Be specific when you can.** "Fix the null pointer in src/auth/login.ts" is faster than "find and fix all bugs"
4. **Use `/clear` between tasks.** Keeps context clean and saves tokens
5. **Use plan mode (`Shift+Tab`) for big changes.** Claude thinks through the approach before writing code
6. **Invest in CLAUDE.md.** A good project config pays for itself immediately
7. **Build skills for things you repeat.** They compound over time
8. **Read the diffs.** Claude shows you what it changed. Always review
9. **Git is your safety net.** Commit before risky operations. You can always revert
10. **Let Claude ask permission.** The guardrails are there to help you

## All Official Documentation Links

- Setup: https://code.claude.com/docs/en/setup
- Quickstart: https://code.claude.com/docs/en/quickstart
- Overview: https://code.claude.com/docs/en/overview
- How It Works: https://code.claude.com/docs/en/how-claude-code-works
- Common Workflows: https://code.claude.com/docs/en/common-workflows
- Best Practices: https://code.claude.com/docs/en/best-practices
- Memory & CLAUDE.md: https://code.claude.com/docs/en/memory
- Skills: https://code.claude.com/docs/en/skills
- Settings: https://code.claude.com/docs/en/settings
- Model Config: https://code.claude.com/docs/en/model-config
- MCP: https://code.claude.com/docs/en/mcp
- Chrome: https://code.claude.com/docs/en/chrome
- Subagents: https://code.claude.com/docs/en/sub-agents
- CLI Reference: https://code.claude.com/docs/en/cli-reference
- Plans & Pricing: https://claude.com/pricing
- VS Code: https://code.claude.com/docs/en/vs-code
- JetBrains: https://code.claude.com/docs/en/jetbrains
- GitHub Actions: https://code.claude.com/docs/en/github-actions
