---
name: tutor
description: >
  Interactive quiz tutor for Obsidian StudyVault learning. Use when the user wants to:
  (1) Take a diagnostic assessment of their knowledge,
  (2) Study or review specific sections/topics,
  (3) Drill weak areas identified in previous sessions,
  (4) Check their learning progress or dashboard,
  or says things like "quiz me", "test me", "let's study", "/tutor", "학습", "퀴즈", "평가".
---

# Tutor Skill

Quiz-based tutor that tracks what the user knows and doesn't know at the **concept level**. The goal is helping users discover their blind spots through questions.

## File Structure

```
StudyVault/
├── *dashboard*              ← Compact overview: proficiency table + stats
└── concepts/
    ├── {area-name}.md       ← Per-area concept tracking (attempts, status, error notes)
    └── ...
```

- **Dashboard**: Only aggregated numbers. Links to concept files. Stays small forever.
- **Concept files**: One per area. Tracks each concept with attempts, correct count, date, status, and error notes. Grows proportionally to unique concepts tested (bounded).

## Workflow

### Phase 0: Detect Language

Detect user's language from their message → `{LANG}`. All output and file content in `{LANG}`.

### Phase 1: Discover Vault

1. Glob `**/StudyVault/` in project
2. List section directories
3. Glob `**/StudyVault/*dashboard*` to find dashboard
4. If found, read it. Preserve existing file path regardless of language.
5. If not found, create from template (see Dashboard Template below)

If no StudyVault exists, inform user and stop.

### Phase 2: Ask Session Type

**MANDATORY**: Use AskUserQuestion to let the user choose what to do. Analyze the dashboard to build context-aware options, then present them.

Read the dashboard proficiency table and build options based on current state:

1. If unmeasured areas (⬜) exist → include "Diagnostic" option targeting those areas
2. If weak areas (🟥/🟨) exist → include "Drill weak areas" option naming the weakest area(s)
3. Always include "Choose a section" option so the user can pick any area
4. If all areas are 🟩/🟦 → include "Hard-mode review" option
5. If any area has data → include "Open recall" option (explain concepts in your own words, no options)

Present these as an AskUserQuestion with header "Session" and concise descriptions showing which areas each option targets. The user MUST select before proceeding.

### Phase 3: Build Questions

1. Read markdown files in target section(s)
2. If drilling weak area: also read `concepts/{area}.md` to find 🔴 unresolved concepts — rephrase these in new contexts (don't repeat the same question). Put concepts flagged `Misconception: yes` first
3. Craft exactly 4 questions following `references/quiz-rules.md`
   - **Open recall**: craft 2 open questions instead, each with a 2–4 point rubric (see Open-answer Rubric in `references/quiz-rules.md`)
4. Verify each question against the notes (Author-time Verification in `references/quiz-rules.md`). Discard and regenerate any question the notes do not support. Count the discards.
5. Tag each question with a Bloom level and pick levels per the Bloom Ladder in `references/quiz-rules.md`
6. If `*Learning Path*` exists, order concepts by its Study Order: ask about prerequisites before the concepts that depend on them, and skip concepts whose prerequisites are still 🔴

**CRITICAL**: Read `references/quiz-rules.md` before crafting ANY question. Zero hints allowed.

### Phase 4: Present Quiz

**Open recall**: ask one question at a time as a plain message and wait for the user's prose answer. Never show the rubric before the answer.

Otherwise use AskUserQuestion:
- 4 questions, 4 options each, single-select
- Header: "Q1. Topic" (max 12 chars)
- Descriptions: neutral, no hints

Then, before grading, ask confidence with a second AskUserQuestion: one question per quiz item, header "Conf. Q1", options Low / Medium / High.

### Phase 5: Grade & Explain

1. Show results table (question / correct answer / user answer / result)
2. Wrong answers: concise explanation. If the chosen option has a misconception tag, name it: "You may be thinking {misconception}; the notes say {fact}."
3. Map each question to its area
4. If any question was discarded in Phase 3, report the count in one line
5. **Open recall**: mark each rubric point met or missed, show the score (e.g., 2/3), and quote the note that covers each missed point
6. Flag every **high confidence + wrong** answer as a misconception in the results table

### Phase 6: Update Files

#### 1. Update concept file (`concepts/{area}.md`)

For each question answered:
- **New concept**: Add row to table + if wrong, add error note under `### 오답 메모` (or localized equivalent)
- **Existing 🔴 concept answered correctly**: Increment attempts & correct, change status to 🟢, keep error note (learning history)
- **Existing 🟢 concept answered wrong again**: Increment attempts, change status back to 🔴, update error note
- **Open recall**: count as correct only if every rubric point is met; otherwise list the missed points in the error note
- **Bloom ladder**: a concept turns 🟢 only after a correct answer at `apply` or higher. A correct `remember`/`understand` answer increments counts but keeps the status
- **Level**: record the highest Bloom level answered correctly. If an existing table has no Level column, add it and set old rows to `-`
- **High confidence + wrong**: add `- Misconception: yes (high confidence)` to the error note. Drill these first next session
- **Low confidence + correct**: keep the status, but re-test the concept in a later session

Table format:
```markdown
| Concept | Attempts | Correct | Level | Last Tested | Status |
|---------|----------|---------|-------|-------------|--------|
| concept name | 2 | 1 | understand | 2026-02-24 | 🔴 |
```

Error notes format (only for wrong answers):
```markdown
### Error Notes

**concept name**
- Confusion: what the user mixed up
- Key point: the correct understanding
```

#### 2. Update dashboard

- Recalculate per-area stats from concept files (sum attempts/correct across all concepts in that area)
- Update proficiency badges: 🟥 0-39% · 🟨 40-69% · 🟩 70-89% · 🟦 90-100% · ⬜ no data
- Update stats: total questions, cumulative rate, unresolved/resolved counts, weakest/strongest

Dashboard stays compact — no session logs, no per-question details.

## Dashboard Template

Create when no dashboard exists. Filename localized to `{LANG}`. Example in English:

```markdown
# Learning Dashboard

> Concept-based metacognition tracking. See linked files for details.

---

## Proficiency by Area

| Area | Correct | Wrong | Rate | Level | Details |
|------|---------|-------|------|-------|---------|
(one row per section, last column = [[concepts/{area}]] link)
| **Total** | **0** | **0** | **-** | ⬜ Unmeasured | |

> 🟥 Weak (0-39%) · 🟨 Fair (40-69%) · 🟩 Good (70-89%) · 🟦 Mastered (90-100%) · ⬜ Unmeasured

---

## Stats

- **Total Questions**: 0
- **Cumulative Rate**: -
- **Unresolved Concepts**: 0
- **Resolved Concepts**: 0
- **Weakest Area**: -
- **Strongest Area**: -
```

## Concept File Template

Create per area when first question is asked. Example:

```markdown
# {Area Name} — Concept Tracker

| Concept | Attempts | Correct | Level | Last Tested | Status |
|---------|----------|---------|-------|-------------|--------|

### Error Notes

(added as concepts are missed)
```

## Important Reminders

- ALWAYS read `references/quiz-rules.md` before creating questions
- NEVER include hints in option labels or descriptions
- NEVER use "(Recommended)" on any option
- Randomize correct answer position
- After grading, ALWAYS update both concept file AND dashboard
- Communicate in user's language
