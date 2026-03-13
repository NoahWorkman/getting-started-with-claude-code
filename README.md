# Getting Started with Claude Code

A practical guide to Claude Code from someone who uses it to build everything: websites, MCP servers, robot dogs, CRMs, AI tools, and more. This is how I actually work, not the marketing version.

I teach a class at [Dirigo Labs](https://www.dirigolabs.org/) (Maine's startup accelerator) on using AI tools to build products. This guide started as my class notes. If you're a founder figuring out how to build faster with AI, this is for you.

> **Not using Claude Code (the CLI)?** See [`PASTE-THIS-INTO-YOUR-AI.md`](PASTE-THIS-INTO-YOUR-AI.md) for a version of this guide you can drop into Claude Desktop, ChatGPT, Grok, or any chat-based AI.

---

## Table of Contents

- [So What Is This Thing?](#so-what-is-this-thing)
- [Getting Started (Seriously, It's Easy)](#getting-started-seriously-its-easy)
- [Your First Conversation](#your-first-conversation)
- [Talk to It Like a Person](#talk-to-it-like-a-person)
- [The Best Practice That Matters Most](#the-best-practice-that-matters-most)
- [How the Folder System Works](#how-the-folder-system-works)
- [CLAUDE.md (Your Project's Brain)](#claudemd-your-projects-brain)
- [Skills (Custom Slash Commands)](#skills-custom-slash-commands)
- [Agents (Your AI Team)](#agents-your-ai-team)
- [Memory (Claude Learns Your Style)](#memory-claude-learns-your-style)
- [MCP Servers (Connecting to the Outside World)](#mcp-servers-connecting-to-the-outside-world)
- [Chrome (Browser Automation)](#chrome-browser-automation)
- [Models (Picking the Right One)](#models-picking-the-right-one)
- [Working with GitHub](#working-with-github)
- [My Public Repos](#my-public-repos)
- [Tips I Learned the Hard Way](#tips-i-learned-the-hard-way)
- [All the Official Links](#all-the-official-links)

---

## So What Is This Thing?

Claude Code is an AI coding partner that lives in your terminal. Not an autocomplete. Not a chatbot bolted onto an IDE. It's Claude with full access to your local machine, and it can:

- Read, write, and edit files across your entire codebase
- Run shell commands (git, npm, docker, whatever you need)
- Search and understand large codebases
- Create commits, branches, and pull requests
- Control Chrome for browser testing
- Connect to external services
- Plan complex changes before touching any code
- Remember what it learned about your project across sessions

The thing that surprised me most? You can just talk to it. Tell it what you want in plain English and it figures out the rest. You don't need to know the "right" way to ask. Just describe what you're trying to do.

Boris Cherny, the engineer who created Claude Code, [posted recently](https://x.com/bcherny/status/2004887829252317325) that he hasn't written any code himself in months -- 100% of his code is written by Claude Code. That's the creator of the tool. I'm not quite there, but it's changed how I work more than any tool I've used.

**Official docs:** [code.claude.com/docs](https://code.claude.com/docs/en/)
**GitHub:** [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code)

---

## Getting Started (Seriously, It's Easy)

### Step 1: Install It

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Or if you prefer Homebrew
brew install --cask claude-code
```

### Step 2: Verify It's There

```bash
claude --version
```

### Step 3: Type `claude`

```bash
claude
```

That's it. It will ask you to log in with your Anthropic account. Once you're authenticated, you're in. You're ready to go.

### What Plan Do I Need?

Claude Code is included with Anthropic's monthly subscription plans starting at the **Pro plan ($20/month)**. No separate purchase, no per-token billing to worry about. Just pick a plan and start building.

Full pricing details: [claude.com/pricing](https://claude.com/pricing)

**Setup docs:** [code.claude.com/docs/en/setup](https://code.claude.com/docs/en/setup)

---

## Your First Conversation

Open a terminal, `cd` into any project folder, and type `claude`. You're now in an interactive session. It's like texting a developer who can see and edit your code in real time.

Try these:

```
> what does this project do?
> explain the architecture to me
> add a hello world route to the server
> fix the TypeScript errors in src/utils.ts
> what files have I changed?
> create a commit with a good message
```

You'll notice Claude reads your files, understands the context, and makes changes directly. It asks before doing anything risky (like deleting files or pushing code). That's by design. You're always in control.

### What Claude Says Back

Here's what an actual conversation looks like. You type a plain English request, Claude tells you what it's going to do, does it, and reports back:

```
You: add a health check endpoint to the express server

Claude: I'll add a health check endpoint. Let me first look at your server setup.
        [reads src/server.ts]
        I see you're using Express with TypeScript. I'll add a /health endpoint
        that returns the server status.
        [edits src/server.ts]
        Done. Added GET /health that returns { status: "ok", uptime: process.uptime() }.
        Want me to test it?
```

That's it. No special syntax. No prompt engineering. Just say what you want.

### Useful Commands While You're In a Session

| Command | What It Does |
|---------|-------------|
| `/help` | See everything available |
| `/cost` | Check token usage for this session |
| `/model` | Switch between models (opus, sonnet, haiku) |
| `/clear` | Start fresh (great between unrelated tasks) |
| `/compact` | Compress the conversation to save context |
| `/memory` | View and edit your project memory |
| `/agents` | View, create, and manage your agents |
| `Shift+Tab` | Toggle plan mode (think before coding) |
| `Esc` | Cancel what Claude is doing |
| `Esc + Esc` | Open rewind menu (undo Claude's changes) |

### Starting from the Command Line

```bash
claude                        # Interactive session
claude "your task here"       # Give it a one-shot task
claude -c                     # Continue your last session
claude -r                     # Resume a specific past session
claude --model opus           # Start with a specific model
claude --chrome               # Enable browser automation
```

**Quickstart guide:** [code.claude.com/docs/en/quickstart](https://code.claude.com/docs/en/quickstart)

---

## Talk to It Like a Person

This might be the most important section in this whole guide.

**You can ask Claude anything.** Not just coding tasks. Ask it questions about itself, about how to use features, about what it can do. It's surprisingly good at explaining itself.

```
> how do I set up an MCP server?
> help me write a custom slash command
> what's the best model for my use case?
> can you explain what subagents are?
> what does this error mean?
> how should I organize this project?
```

If you're stuck, confused, or don't know where to start on something, just describe the problem. Claude will either help you solve it or help you figure out the right approach. Don't overthink the prompt. Talk to it like you'd talk to a colleague.

### Have Claude Interview You

This is one of the best patterns from [Anthropic's official best practices](https://code.claude.com/docs/en/best-practices). Instead of trying to write the perfect prompt, let Claude ask YOU the questions:

```
> I want to build a customer onboarding flow. Interview me about the
  requirements. Ask about edge cases, technical constraints, and UX
  decisions I might not have thought about. Keep going until we've
  covered everything, then write a spec.
```

Claude will ask you 5-10 targeted questions -- things you wouldn't have thought to specify upfront. By the end, you'll have a clear spec without having to write it from scratch.

The more you work with it, the more natural it gets. You'll stop thinking of it as a tool and start thinking of it as a collaborator.

---

## The Best Practice That Matters Most

According to Anthropic's own team, the single highest-leverage thing you can do is **give Claude a way to verify its own work**.

```
# Instead of this:
> implement email validation

# Do this:
> implement email validation. test cases: user@example.com = valid,
  "invalid" = invalid, user@.com = invalid. Write the tests first,
  then implement, then run the tests.
```

When Claude can run tests, compare screenshots, or check its output against expected results, it catches its own mistakes. Without verification, you become the only feedback loop, and every error requires your attention.

This applies to everything:
- **Code changes**: "run the tests after you make the change"
- **UI work**: "take a screenshot and compare it to the design"
- **Debugging**: "here's the error. fix it and verify the build succeeds"

**Best practices docs:** [code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)

---

## How the Folder System Works

Claude Code uses a simple folder structure to organize everything: your settings, project memory, skills, and agents. Here's where things live:

### The Global Folder

```
~/.claude/
  CLAUDE.md              # Your personal instructions (loaded in every session)
  settings.json          # Permissions, model preferences, etc.
  skills/                # Your custom slash commands (available in all projects)
    commit-push/
      SKILL.md
    review/
      SKILL.md
  agents/                # Your custom agents (available in all projects)
    code-reviewer.md
  commands/              # Legacy command format (still works)
  projects/              # Auto-memory per project (Claude manages this)
    <project-hash>/
      memory/
        MEMORY.md
```

This lives in your home directory and applies everywhere. Your personal preferences, your favorite skills, your global agents.

### The Project Folder

```
your-project/
  CLAUDE.md              # Shared project config (commit to git for your team)
  CLAUDE.local.md        # Personal project overrides (auto-gitignored)
  .claude/
    skills/              # Project-specific skills
    agents/              # Project-specific agents
    CLAUDE.md            # Alternative location for project config
```

This lives inside each project. Anything here is specific to that codebase.

### How They Stack

More specific always wins. If your global CLAUDE.md says "use tabs" but the project CLAUDE.md says "use spaces," the project wins. Think of it as layers:

1. **Global** (`~/.claude/CLAUDE.md`) - your personal defaults
2. **Project** (`./CLAUDE.md`) - team-shared conventions
3. **Local** (`./CLAUDE.local.md`) - your personal overrides for this project

You don't need to set up all of this on day one. Start with just typing `claude` in a project folder and add configuration as you need it.

---

## CLAUDE.md (Your Project's Brain)

This is the single most impactful thing you can set up. A `CLAUDE.md` file tells Claude how to work in a specific codebase. It gets loaded at the start of every session.

### What Goes in It

- Build, test, and lint commands
- Architecture overview
- Coding style and conventions
- File organization patterns
- Things Claude should never do in this project
- Links to key files

### What Does NOT Belong

- Things Claude already knows from reading your code (it can figure out your framework)
- Standard language conventions (Claude knows Python style, JS conventions, etc.)
- Long tutorials or API documentation (link to docs instead)
- File-by-file descriptions of the codebase

Keep it concise. If your CLAUDE.md is too long, Claude starts ignoring parts of it. For each line, ask: "Would removing this cause Claude to make mistakes?" If not, cut it.

### Creating One

The easiest way:

```bash
cd your-project
claude
> /init
```

Claude will scan your codebase and generate a starter CLAUDE.md. You can also create one manually, which I actually recommend once you know your project well. You know it better than an auto-scan does.

See [`examples/CLAUDE.md.example`](examples/CLAUDE.md.example) for a starter template.

### Why It Matters

Without a CLAUDE.md, every session starts cold. Claude has to re-learn your project's quirks. With a good CLAUDE.md, it knows your conventions, your toolchain, and your preferences from the first message. 30 minutes writing one saves hours of re-explaining.

**Memory docs:** [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)

---

## Skills (Custom Slash Commands)

This is where things get fun. You can create custom commands that Claude executes with specialized instructions. Think of them as reusable recipes for things you do all the time.

### How It Works

Create a markdown file. The filename becomes the command. The contents become Claude's instructions.

### Where They Live

```
~/.claude/skills/<skill-name>/SKILL.md    # Personal, all projects
.claude/skills/<skill-name>/SKILL.md      # Project-specific, committed to git
```

### Example: A Commit Skill

Create `~/.claude/skills/commit/SKILL.md`:

```markdown
---
name: commit
description: Validate, commit, and push changes
---

1. Run the project's lint/format/test commands
2. Check git status and diff
3. Create a descriptive commit message
4. Commit and push
```

Now `/commit` runs your custom workflow every time. It runs your tests, writes a commit message, and pushes. One command.

### Writing Good Skills (Not Just Task Lists)

The best skills don't just list steps. They teach Claude *how to think* about the task. Compare:

**Weak** (a checklist):
```
1. Run git status
2. Stage all changes
3. Commit
4. Push
```

**Strong** (behavioral guidance):
```
Stage files by name -- never use `git add .` (can include secrets).
If you see .env files or credentials in the diff, warn the user.
Write a commit message that explains WHY, not just what changed.
If validation fails, stop. Do not commit broken code.
```

The first version tells Claude what buttons to press. The second teaches it what to care about. Skills that teach decisions age better than skills that prescribe steps.

### My Setup

I have **34 custom slash commands**. Some highlights:

| Command | What It Does |
|---------|-------------|
| `/commit-push` | Validate, commit, and push |
| `/prd` | Generate a full product requirements doc |
| `/contract-review` | Analyze a contract for red flags |
| `/retro` | Run a conversation retrospective |
| `/record-gifs` | Record browser interaction GIFs |
| `/contrast-check` | Audit CSS color contrast for accessibility |

Skills compound over time. Every one you build saves time in every future session. Start with the tasks you repeat most often.

Browse more examples in my [claude-skills](https://github.com/NoahWorkman/claude-skills) repo.

**Skills docs:** [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

---

## Agents (Your AI Team)

Agents are specialized AI workers that Claude can spin up to handle specific tasks. Each one gets its own context, its own tools, and its own job. Think of them as members of a team with different roles.

### Built-in Agents

Claude comes with these out of the box:

| Agent | What It Does |
|-------|-------------|
| **Explore** | Fast, read-only codebase search and analysis |
| **Plan** | Research and planning (reads code, doesn't change it) |
| **General-purpose** | Complex multi-step tasks with full access |

You don't call these directly. Claude delegates to them automatically when it makes sense.

### Creating Your Own Agents

The easiest way:

```
/agents
# Select "Create new agent" > "User-level"
# Describe what the agent should do
# Choose tools and model
# Done
```

Or create them as markdown files in `~/.claude/agents/` (personal) or `.claude/agents/` (project-specific).

### Example: A Code Reviewer Agent

Create `~/.claude/agents/code-reviewer.md`:

```markdown
---
name: code-reviewer
description: Reviews code for quality and best practices. Use proactively after code changes.
tools: Read, Glob, Grep, Bash
model: sonnet
---

You are a senior code reviewer. When invoked:
1. Run git diff to see recent changes
2. Review for bugs, security issues, style problems
3. Format feedback by priority: critical, warnings, suggestions
```

### Why Agents Are Great

- **They stay out of your way.** Verbose tasks (test suites, log analysis) run in their own context instead of cluttering your main conversation
- **You control their access.** A reviewer agent can be read-only. A deployment agent can have full access. You decide
- **They run in parallel.** Claude can spin up multiple agents working on different things at the same time
- **They're resumable.** Press `Ctrl+B` to background a running agent and keep working. Come back to it later

**Subagent docs:** [code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents)

---

## Memory (Claude Learns Your Style)

Claude Code remembers things across sessions. Not just what you tell it, but patterns it discovers while working with your codebase.

### How It Works

Claude maintains a memory folder for each project:

```
~/.claude/projects/<project-hash>/memory/
  MEMORY.md          # Main index (loaded every session)
  debugging.md       # Topic-specific notes
  patterns.md        # Patterns discovered in your code
```

### What Gets Remembered

- Debugging insights ("this error means X, fix it by doing Y")
- Architecture patterns it discovered
- Your preferences it learned from your corrections
- Facts about the project
- Lessons from mistakes it made

### Managing Memory

```
/memory              # View and edit memory files during a session
```

### Why This Matters

Without memory, every session starts from zero. With memory, Claude gets better at your specific project over time. It won't repeat the same mistakes. It'll follow your conventions. It'll know the gotchas before you have to remind it.

This is one of the things that makes Claude Code feel less like a tool and more like a partner that actually knows your project.

**Memory docs:** [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)

---

## MCP Servers (Connecting to the Outside World)

MCP (Model Context Protocol) lets Claude connect to external services and tools. Instead of copy-pasting data between Claude and other apps, Claude can reach out directly.

### What Can MCPs Do?

- Query databases
- Call APIs
- Control hardware (I have one for a robot dog)
- Read from project management tools
- Interact with cloud services
- Access your calendar, email, and files

### Installing One

```bash
# Remote HTTP server
claude mcp add --transport http --scope user <name> <url>

# Remote SSE server
claude mcp add --transport sse --scope user <name> <url>

# Local npm package
claude mcp add --transport stdio --scope user <name> -- npx -y <package>
```

Use `--scope user` to make it available across all your projects.

### Managing MCPs

```bash
claude mcp list              # See all connected MCPs
claude mcp remove <name>     # Remove one
/mcp                         # Check status during a session
```

MCPs are optional and you definitely don't need them to get started. But once you start connecting services, it's a game-changer. Claude goes from "a tool that works with your code" to "a tool that works with everything."

**MCP docs:** [code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp)

---

## Chrome (Browser Automation)

Claude can control your Chrome browser. This one surprised me.

### Setup

1. Install the **Claude in Chrome** extension from the Chrome Web Store
2. Launch with: `claude --chrome` (or type `/chrome` during a session)

### What It Can Do

- Navigate pages, click elements, fill forms
- Read page content and console logs
- Take screenshots and record GIFs
- Test your web app end-to-end
- Interact with authenticated pages (it uses your real browser session)

### Examples

```
> go to localhost:3000 and verify the dashboard loads
> open this URL and extract all the pricing info into a table
> navigate through the signup flow and record a GIF
```

You can enable it by default with `/chrome` and selecting "Enabled by default."

**Chrome docs:** [code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome)

---

## Models (Picking the Right One)

Claude Code gives you three models. You don't need to worry about per-token costs on a subscription plan, just pick the right one for the job.

| Model | Best For |
|-------|---------|
| **Opus 4.6** | Complex reasoning, architecture, multi-file refactors |
| **Sonnet 4.5** | Most daily coding (this is your workhorse) |
| **Haiku 4.5** | Quick lookups, simple edits, fast iteration |

### Switching

```bash
# At startup
claude --model sonnet

# During a session
/model opus

# The power move: Opus for thinking, Sonnet for doing
claude --model opusplan
```

The `opusplan` mode uses Opus when you're in plan mode (`Shift+Tab`) for deep thinking, then automatically drops to Sonnet when executing. Best of both worlds.

### My Recommendation

Start with **Sonnet**. It handles 90% of tasks. Switch to **Opus** when you're dealing with complex architecture or tricky debugging. Use **Haiku** when you're iterating quickly on small changes.

**Model docs:** [code.claude.com/docs/en/model-config](https://code.claude.com/docs/en/model-config)

---

## Working with GitHub

Claude Code has deep GitHub integration through the `gh` CLI.

### The Basics

```bash
gh repo clone your-username/your-repo
cd your-repo
claude
```

Claude automatically detects it's a git repo and understands the full history, branches, and remote.

### My Typical Workflow

```
> create a branch called feature/new-auth
> implement JWT authentication with refresh tokens
> show me a summary of all changes
> commit these changes with a descriptive message
> push this branch and create a PR
```

### PR Workflows

```bash
# Review a PR
claude --from-pr 123

# Work on someone else's PR
gh pr checkout 456
claude
> what does this PR change?
```

---

## My Public Repos

| Repo | What It Is |
|------|-----------|
| [claude-skills](https://github.com/NoahWorkman/claude-skills) | Reusable Claude Code slash command library |
| [mcp-ffmpeg](https://github.com/NoahWorkman/mcp-ffmpeg) | MCP server for FFmpeg media inspection and transformation |
| [petoi-bittle-project](https://github.com/NoahWorkman/petoi-bittle-project) | Robot dog: voice commands, Bluetooth, AI vision, custom MCP server |

---

## Tips I Learned the Hard Way

1. **Just talk to it.** Don't stress about prompt engineering. Describe what you want like you'd tell a coworker. Claude is very good at figuring out what you mean

2. **Ask questions.** Stuck on something? Ask Claude. It can explain code, suggest approaches, debug errors, and walk you through concepts. You're not bothering it

3. **Give it a way to check its work.** Tell Claude to run tests, take screenshots, or verify the build passes after making changes. This is Anthropic's #1 best practice and it makes a huge difference

4. **Be specific when you can.** "Fix the bug" works, but "fix the null pointer in src/auth/login.ts line 42" is faster and cheaper

5. **Use `/clear` between tasks.** Context builds up. If you're done with one thing and starting another, clear it. After two failed corrections on the same problem, clear and start fresh with a better prompt

6. **Plan mode for big changes.** Hit `Shift+Tab` before asking for a major refactor. Claude will explore the codebase, create a plan, and get your approval before writing code

7. **CLAUDE.md is worth the investment.** 30 minutes writing a good one saves hours of re-explaining conventions. But keep it short -- a bloated CLAUDE.md gets ignored

8. **Skills compound.** Every slash command you build saves time on every future session. Start with what you repeat most. Teach the skill *how to decide*, not just what to do

9. **Read the diff.** Claude shows you what it changed. Actually read it. It's right the vast majority of the time, but you should always review

10. **Git is your safety net.** Claude works on real files. Commit before asking it to do something risky. Use `Esc+Esc` to rewind if something goes wrong

11. **Let it ask permission.** Claude checks before doing risky things (deleting files, pushing code). That's a feature, not a bug. Let the guardrails work for you

12. **Use subagents for research.** If you need Claude to explore a large codebase, say "use a subagent to investigate X." It runs in a separate context so your main conversation stays clean

---

## All the Official Links

### Getting Started
- [Installation & Setup](https://code.claude.com/docs/en/setup)
- [Quickstart Guide](https://code.claude.com/docs/en/quickstart)
- [Overview](https://code.claude.com/docs/en/overview)
- [How Claude Code Works](https://code.claude.com/docs/en/how-claude-code-works)
- [Common Workflows](https://code.claude.com/docs/en/common-workflows)
- [Best Practices](https://code.claude.com/docs/en/best-practices)

### Configuration
- [CLAUDE.md & Memory](https://code.claude.com/docs/en/memory)
- [Skills (Custom Commands)](https://code.claude.com/docs/en/skills)
- [Settings & Permissions](https://code.claude.com/docs/en/settings)
- [Model Configuration](https://code.claude.com/docs/en/model-config)
- [MCP Integration](https://code.claude.com/docs/en/mcp)

### Advanced
- [Chrome Integration](https://code.claude.com/docs/en/chrome)
- [Subagents](https://code.claude.com/docs/en/sub-agents)
- [CLI Reference](https://code.claude.com/docs/en/cli-reference)

### IDE Integrations
- [VS Code Extension](https://code.claude.com/docs/en/vs-code)
- [JetBrains IDEs](https://code.claude.com/docs/en/jetbrains)
- [GitHub Actions](https://code.claude.com/docs/en/github-actions)

### Pricing
- [Plans & Pricing](https://claude.com/pricing)
- [Using Claude Code with Your Plan](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)

### Community
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Noah's Claude Skills Library](https://github.com/NoahWorkman/claude-skills)
- [Awesome Claude Code (Community)](https://github.com/hesreallyhim/awesome-claude-code)

---

## Questions?

Honestly, just ask Claude:

```bash
claude
> how do I set up an MCP server?
> help me write a custom slash command
> what's the best model for my use case?
```

It's surprisingly good at explaining itself. And if you need to reach me directly, you know where to find me.
