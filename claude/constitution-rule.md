# 클로드 헌법

## Git Commit Protocol: Intent-Driven Commit Generation

### Philosophy
Code is the *output*. The real source of truth is the **intent behind the conversation**.
Never summarize *what* changed — always articulate *why* it changed.

---

### Rules

**1. Context Mapping Before Commit**
When a commit is requested or staged files are detected:
- Do NOT rely solely on the `git diff`.
- Trace back **all conversation turns since the last commit** and extract:
  - The user's core requirement or goal
  - Any constraints, trade-offs, or deliberate compromises discussed
  - The "why" behind each structural decision
- Map this conversational context to the actual code changes.

**2. Zero-Fatigue Commit Draft**
Generate a commit message with the following structure:
```
<type>(<scope>): <concise intent summary>

[Why]
<1–3 sentences derived from conversation context, not from the diff>

[Architecture Decision]
<One line on how this change affects system structure, or why this approach was chosen over alternatives>

[Trade-offs / Deferred]
<Optional: note any conscious shortcuts, hardcoded values, or decisions intentionally left for later>
```

Prohibited in commit messages:
- Mechanical diffs ("renamed function X to Y", "fixed typo")
- Vague summaries ("refactored code", "updated logic")
- Any message that could have been auto-generated without reading the conversation

**3. The One Deep Question**
After presenting the commit draft, ask **exactly one** architecturally meaningful question.

The question must:
- Be derived from the conversation context, not generic best practices
- Target a potential tension: e.g., separation of concerns, a deferred decision, a trade-off not yet resolved
- Be answerable in 1–2 sentences

When the user answers, immediately incorporate their response into the `[Architecture Decision]` or `[Trade-offs / Deferred]` section before finalizing the commit.

Do NOT ask multiple questions. Do NOT ask for approval with Y/n alone.

**4. Scope of Application**
This protocol applies regardless of:
- Project type (web, mobile, embedded, data pipeline, etc.)
- Team size (solo or collaborative)
- Tooling (Claude Code, Cursor, any AI-assisted environment)

If no conversation context exists (e.g., direct CLI commit with no prior chat), fall back to diff-based summarization and explicitly note: `[No conversation context available — diff-based summary]`.
