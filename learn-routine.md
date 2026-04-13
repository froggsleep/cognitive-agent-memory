# Learn Routine

Use this as a session-end prompt. Copy it into a slash command, automation hook, or just paste it when you're done working.

---

```
Save session memory now. Follow these steps in order:

1. **Episode** — Create `memory/episodes/YYYY-MM-DD-session-N.md`
   - Summarize: what you did, decisions made, things learned
   - Use bullet points, keep it scannable

2. **Semantic** — If you learned new project/domain knowledge, update files in `memory/semantic/`

3. **Normative** — If the user corrected you or gave feedback, update `memory/norms/`

4. **Procedural** — If you confirmed a repeatable workflow (2+ times), add to `memory/procedures/`

5. **Index** — Update `memory/MEMORY.md` if any new files were created

6. **Report** — Tell me what you saved/updated (brief)
```

---

## Tips

- Run this at the end of every meaningful session (not after quick one-off questions).
- If using Claude Code, you can save this as a custom slash command or a hook that runs on session end.
- The agent should assess: "Did anything non-obvious happen this session that future-me would benefit from knowing?"
- Keep episode files factual and concise. 20-30 lines is ideal.
