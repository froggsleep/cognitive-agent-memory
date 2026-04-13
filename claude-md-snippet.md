# CLAUDE.md Memory System Snippet

Copy the section below into your agent's `CLAUDE.md` (or `AGENTS.md`, depending on your framework).

---

```markdown
## Memory

Sessions reset every time — files are your only continuity.

### Session Startup

Before doing anything else:

1. Read `memory/MEMORY.md` for long-term context
2. Check `memory/episodes/` for recent activity (today + yesterday)
3. Load relevant semantic/procedural memories for the current task.

### Structure
```
memory/
├── MEMORY.md              — Index (< 200 lines, always loaded)
├── episodes/              — Session logs (YYYY-MM-DD-session-N.md)
│   └── archive/           — Older episodes
├── semantic/              — Project/domain knowledge
├── procedures/            — Verified workflows (2+ repetitions)
├── norms/                 — Rules and constraints from user feedback
├── user_role.md           — User profile
└── reference_tools.md     — External system pointers
```

### What to Record
- **Record:** Decisions and rationale, user corrections, non-obvious solutions, project constraints
- **Skip:** Things derivable from code, git history, one-time debugging details, info already in docs

### Write It Down — No "Mental Notes"
Mental notes don't survive session restarts. Files do.
"Remember this" → write to a file immediately.

### Self-Assessment (after each task)
- "Did I discover a non-obvious solution?"
- "Did I learn a project-specific behavior/constraint?"
- "Did the user correct me?"
→ If yes, write to the appropriate memory file immediately.

### Learn Routine (end of session)
1. Create/update episode in `memory/episodes/YYYY-MM-DD-session-N.md`
2. Update semantic/normative/procedural memories as needed
3. Update `memory/MEMORY.md` index
4. Report what was saved
```

---

## Notes

- The `memory/` directory should be inside your agent's workspace (the directory used as `cwd`).
- `MEMORY.md` is the only file that MUST be read every session. Everything else is loaded on demand.
- Adjust the structure to your needs — the 6 types are a framework, not a rigid schema.
- For Claude Code: place this in the project-level `CLAUDE.md` or user-level `~/.claude/CLAUDE.md`.
