# 클로드 헌법

## Git Commit Protocol: Intent-Driven Commit Generation

### Language Rule
All outputs (commit messages, questions, responses) must be written in **Korean (한국어)**.
Prompt rules are written in English for AI precision, but every output facing the user must be in Korean.

---

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
<타입>(<범위>): <핵심 의도 요약>

[왜 변경했는가]
<대화 컨텍스트에서 추출한 1~3문장. diff 요약 금지>

[아키텍처 결정]
<이 변경이 시스템 구조에 미치는 영향, 또는 다른 방법 대신 이 방식을 선택한 이유 한 줄>

[트레이드오프 / 보류 사항]
<선택적: 의도적인 단순화, 하드코딩, 또는 다음 단계로 미룬 결정 명시>
```

Prohibited in commit messages:
- Mechanical diffs ("함수명 X를 Y로 변경", "오타 수정")
- Vague summaries ("코드 리팩터링", "로직 업데이트")
- Any message that could have been auto-generated without reading the conversation

**3. The One Deep Question**
After presenting the commit draft, ask **exactly one** architecturally meaningful question.

The question must:
- Be derived from the conversation context, not generic best practices
- Target a potential tension: e.g., separation of concerns, a deferred decision, a trade-off not yet resolved
- Be answerable in 1–2 sentences

When the user answers, immediately incorporate their response into the `[아키텍처 결정]` or `[트레이드오프 / 보류 사항]` section before finalizing the commit.

Do NOT ask multiple questions. Do NOT ask for approval with Y/n alone.

**4. Scope of Application**
This protocol applies regardless of:
- Project type (web, mobile, embedded, data pipeline, etc.)
- Team size (solo or collaborative)
- Tooling (Claude Code, Cursor, any AI-assisted environment)

If no conversation context exists (e.g., direct CLI commit with no prior chat), fall back to diff-based summarization and explicitly note:
`[대화 컨텍스트 없음 — diff 기반 요약으로 대체]`
