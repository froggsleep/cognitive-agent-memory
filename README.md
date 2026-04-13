# Cognitive Agent Memory

A file-based, persistent memory system for AI agents, inspired by cognitive engineering principles.

> "Memory is not storage — it's **reconstruction**."
> The goal is not to save everything, but to surface the right memory at the right time.

## Quickstart

```bash
# 1. Create memory directory in your agent's workspace
mkdir -p memory/{episodes/archive,semantic,procedures,norms}

# 2. Copy the starter files
cp templates/MEMORY.md memory/MEMORY.md
cp templates/normative.md memory/norms/communication.md
cp templates/user.md memory/user_role.md

# 3. Add the memory system config to your CLAUDE.md
#    (copy the block from claude-md-snippet.md)

# 4. Start a session — the agent reads MEMORY.md and begins building memory
```

The agent discovers its memory on session start, records learnings during work, and saves at session end using the learn routine in `learn-routine.md`.

## Why

AI agents reset every session. Without persistent memory, each conversation starts from zero — the agent re-learns preferences, and repeats past mistakes. This system gives agents durable memory across sessions using nothing but files.

**No database. No vector store. No external service.** Just markdown files in a directory. The index (`MEMORY.md`) is loaded every session; other files are loaded on demand based on context.

## The 6 Memory Types

Modeled after human cognitive memory systems:

| Type | What it stores | Lifespan | Human analogy |
|------|---------------|----------|---------------|
| **Episodic** | "When did I do what?" — session logs, work history | Medium-term (recent active, older archived) | Remembering what you did last Tuesday |
| **Semantic** | "How does the world work?" — project architecture, domain knowledge | Permanent (updated on change) | Knowing that Python uses indentation |
| **Procedural** | "How do I do this?" — verified workflows, step-by-step processes | Permanent (only proven patterns) | Riding a bicycle |
| **Normative** | "What should/shouldn't I do?" — rules, constraints, user feedback | Permanent (owner-controlled) | Knowing not to interrupt people |
| **User** | Who is the user? Role, preferences, context | Permanent | Knowing your colleague's expertise |
| **Reference** | Where to find things — pointers to external systems | Permanent | Knowing which shelf the dictionary is on |

## Directory Structure

```
memory/
├── MEMORY.md              # Index file (< 200 lines) — always loaded
├── episodes/              # Episodic: session & work logs
│   ├── 2026-04-10-session-1.md
│   └── archive/           # Older episodes
├── semantic/              # Semantic: project knowledge, architecture
│   ├── project-alpha.md
│   └── project-beta.md
├── procedures/            # Procedural: verified workflows
│   ├── deploy-workflow.md
│   └── code-review-flow.md
├── norms/                 # Normative: rules, feedback, constraints
│   ├── communication.md
│   └── work-style.md
├── user_role.md           # User: who am I working with
└── reference_tools.md     # Reference: external system pointers
```

## How It Works

### Session Start
1. Agent reads `MEMORY.md` (the index — always loaded into context)
2. Checks `episodes/` for recent activity (today + yesterday)
3. Loads relevant semantic/procedural/normative memories based on the task

### During Session
- New user feedback → immediately save to `norms/`
- Decision made → note in episodic buffer
- Project structure learned → update `semantic/`
- Repeated pattern confirmed → promote to `procedures/`

### Session End (`/learn` routine)
1. Create episodic record in `episodes/YYYY-MM-DD-session-N.md`
2. Update semantic/normative/procedural memories as needed
3. Update `MEMORY.md` index
4. Self-assess: "Did I learn something non-obvious? Did the user correct me?"

## What to Save vs Skip

### Save
- Decisions and their rationale (why, not just what)
- User corrections and feedback (prevents repeating mistakes)
- Non-obvious solutions (things you wouldn't derive from code alone)
- Project-specific constraints (deploy rules, naming conventions)
- Relationship context (who owns what, who to ask)

### Skip
- Anything derivable from code (file structure, function signatures — `git` is the source of truth)
- Git history (use `git log`, don't duplicate it)
- One-time debugging details (error messages, stack traces)
- Information already in project docs (CLAUDE.md, README)

## Memory File Templates

### Episodic (Session Record)
```markdown
---
date: 2026-04-10
session: 1
projects: [project-alpha]
---

## Work Done
- Implemented feature X
- Fixed bug in module Y

## Decisions
- Chose approach A over B because [reason] → [outcome]

## Learned
- Module Y has an undocumented dependency on Z

## Next Actions
- [ ] Deploy feature X to staging
```

### Semantic (Project Knowledge)
```markdown
---
name: project-alpha
description: E-commerce API — architecture and current state
type: semantic
---

## Architecture
[Key modules and their relationships]

## Current State
[What's deployed, what's in progress]

## Gotchas
[Non-obvious things that trip people up]
```

### Procedural (Workflow)
```markdown
---
name: deploy-workflow
description: How to deploy to production
type: procedural
---

## When to Use
Feature branch merged to main, all tests passing

## Steps
1. Run `npm run build`
2. Run test suite
3. Tag release
4. Deploy via CI

## Watch Out For
- Database migrations must run before deploy
- Clear CDN cache after frontend changes
```

### Normative (Rule / Feedback)
```markdown
---
name: communication-style
description: How to communicate with the user
type: normative
---

## Rules
- Use formal language (not casual)
- Be concise — lead with the answer, not the reasoning
- Don't speculate — verify before answering

## Why
User feedback on 2026-03-23: "Too verbose, stop guessing"

## When to Apply
All messages, especially Discord and Slack
```

## The Index: MEMORY.md

The index is the **only file guaranteed to be loaded every session**. Keep it under 200 lines. Each entry is a one-line pointer:

```markdown
# Memory Index

## User
- [user_role.md](user_role.md) — Senior engineer, focused on backend

## Normative (norms/)
- [norms/communication.md](norms/communication.md) — Formal language, concise, verify before answering
- [norms/work-style.md](norms/work-style.md) — Autonomous, investigate before proposing

## Semantic (semantic/)
- [semantic/project-alpha.md](semantic/project-alpha.md) — E-commerce API architecture

## Procedural (procedures/)
- [procedures/deploy-workflow.md](procedures/deploy-workflow.md) — Production deploy steps

## Episodic (episodes/)
- [episodes/2026-04-10-session-1.md](episodes/2026-04-10-session-1.md) — Implemented auth module
```

## 3 Core Mechanisms

### 1. Meta-cognition ("knowing what you know")
- Session start: scan index → decide what's relevant to load
- During work: "Do I already know this, or should I check memory?"
- Session end: "What did I learn that future-me needs?"

### 2. Lifecycle Management
```
Formation  → New feedback, completed work, structural discovery
Consolidation → Repeated patterns promoted to procedures
Decay      → 7+ day old episodes archived
Deletion   → Owner says "forget this" / info proven wrong
```

### 3. Conflict Resolution
- **Memory vs current code**: code wins → update memory
- **Memory vs user instruction**: user wins → update immediately
- **Old memory vs new memory**: newest wins → archive old
- **Uncertain memory**: verify before asserting as fact

## CLAUDE.md Integration

Add this to your agent's `CLAUDE.md` to activate the memory system:

```markdown
## Memory

Session resets every time. Files are your only continuity.

### Session Startup
1. Read `memory/MEMORY.md` for long-term context
2. Check `memory/episodes/` for recent activity (today + yesterday)
3. Load relevant semantic/procedural memories for the current task

### What to Record
- **Record:** Decisions, user feedback, non-obvious solutions, project constraints
- **Skip:** Things derivable from code, git history, one-time details

### Self-Assessment (after each task)
- "Did I discover a non-obvious solution?"
- "Did I learn a project-specific behavior/constraint?"
- "Did the user correct me?"
→ If yes, write to the appropriate memory file immediately

### Memory Structure
memory/
├── MEMORY.md          — Index (< 200 lines)
├── episodes/          — Session logs (YYYY-MM-DD-session-N.md)
├── semantic/          — Project/domain knowledge
├── procedures/        — Verified workflows (2+ repetitions)
├── norms/             — Rules from user feedback
├── user_role.md       — User profile
└── reference_tools.md — External system pointers

### Write It Down — No "Mental Notes"
Mental notes don't survive session restarts. Files do.
When someone says "remember this" → write to a file immediately.
```

## Operational Lessons

After months of running this system with multiple agents:

1. **The index is everything.** If it's not in `MEMORY.md`, the agent won't find it. Keep the index maintained.

2. **Norms are the highest-value memory type.** A single "don't do X" rule prevents the same mistake across hundreds of sessions.

3. **Episodic records are surprisingly useful.** "What did we do last Tuesday?" is a real question. Date-stamped session logs answer it.

4. **Don't over-save.** The temptation is to record everything. Resist. Noise drowns signal. If the information is in the code, don't duplicate it in memory.

5. **Memory files should be updated, not appended.** A semantic memory file should reflect the *current* state of a project, not a changelog of every state it's been in.

6. **The `/learn` routine is non-negotiable.** Without an explicit session-end save step, memory drift is inevitable. Automate the prompt.

7. **Agents forget what's not in files.** "I'll remember this for next time" is a lie. If you didn't write it down, it's gone.

## License

MIT. Use it however you want.

## Contributing

This is a design pattern, not a library. If you adapt it for your own agents and discover improvements, open an issue or PR describing what you changed and why.
