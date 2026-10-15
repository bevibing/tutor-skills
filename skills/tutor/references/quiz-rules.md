# Quiz Design Rules

## Zero-Hint Policy (CRITICAL)

Every question must be answerable ONLY by someone who actually knows the material.

1. **Option descriptions**: NEVER reveal correctness
   - BAD: `label: "stderr"`, `description: "Error output stream used by Cloud Run for error classification"`
   - GOOD: `label: "stderr"`, `description: "Standard error stream"`

2. **No "(Recommended)" tag** on any option

3. **Randomize** correct answer position — never always first or last

4. **Question phrasing**: Ask about behavior/purpose/output, don't hint at the answer
   - BAD: "Which error stream does error() use?"
   - GOOD: "Where does error() method output go?"

5. **Plausible distractors**: Wrong options must be real concepts from the domain, representing common misconceptions
   - Tag each wrong option with the misconception it targets (e.g., `misconception: "422 is for malformed JSON"`)
   - Keep tags in your working notes only — NEVER put them in labels or descriptions
   - Reuse confusions from `### Error Notes` in concept files when available

## Author-time Verification (CoVe)

After crafting each question and before presenting it:

1. Answer the question using ONLY the section notes read in Phase 3
2. Check that the notes support exactly one correct option and rule out every distractor
3. If not, discard the question and write a new one

Never present a question that needs knowledge outside the vault to answer.

## Question Types

1. **Factual recall**: "What HTTP status code is returned when...?"
2. **Conceptual understanding**: "Why does the system use X pattern?"
3. **Behavioral prediction**: "What happens when X fails?"
4. **Comparison/distinction**: "What is the difference between X and Y?"
5. **Debugging scenario**: "Given this error, what is the most likely cause?"

## Open-answer Rubric

For "Open recall" sessions:

- Ask "explain", "why", or "what happens when" questions — not one-word facts
- Write 2–4 rubric points per question, each a key idea stated in the notes
- Grade by meaning, not keywords — a correct paraphrase meets the point
- Every rubric point must cite a vault note; drop points the notes do not cover
- Keep the rubric hidden until the user has answered

## Difficulty Balancing

- Diagnostic: easy 40%, medium 40%, hard 20%
- Weak-area drill: medium 30%, hard 70%
- Review: all levels evenly

## Drilling Unresolved Concepts

When targeting 🔴 concepts from concept files:
- Do NOT repeat the exact same question — rephrase in a new context
- Test the same underlying knowledge from a different angle
- E.g., if user confused "400 vs 422", ask a scenario question where they must choose the correct status code for a new situation

## AskUserQuestion Format

- 4 questions per round, 4 options each, single-select
- Header: max 12 chars, "Q1. Topic"

## File Update Protocol

After grading:
1. Update `concepts/{area}.md` — add/update concept rows + error notes
2. Update dashboard — recalculate area stats from concept files
3. Badges: 🟥 0-39% · 🟨 40-69% · 🟩 70-89% · 🟦 90-100% · ⬜ no data

## Language Rule

All file content and output in the user's detected language. Badge emojis are universal.
