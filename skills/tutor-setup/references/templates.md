# Templates Reference

## Vault Folder Structure

```
StudyVault/
  00-Dashboard/          # MOC + cheat sheets + Exam Traps
  01-<Topic1>/           # Concept notes per domain
  02-<Topic2>/
  ...
  NN-문제풀이/ (or Practice/)
```

## Dashboard MOC Template

```markdown
---
source_pdf: <list all source files>
part: <part numbers or "all">
keywords: MOC, study map, <subject>
---

# <Subject> Study Map

#dashboard #<subject-tag>

## Overview
- Exam/certification info (if applicable)
- Domain weights or topic importance

## Topic Map
| Section | Source | Notes | Status |
|---------|--------|-------|--------|
| Topic 1 | Part 1 | [[Note 1]], [[Note 2]] | [ ] |

## Practice Notes
| 문제셋 | 문항 수 | 링크 |
|--------|---------|------|
| Topic 1 | N문제 | [[Topic 1 Practice]] |

## Study Tools
| 도구 | 설명 | 링크 |
|------|------|------|
| Exam Traps | 시험 함정/오답 포인트 모음 | [[Exam Traps]] |
| Quick Reference | 전체 치트시트 | [[빠른 참조]] |

## Tag Index
| Tag | 관련 주제 | 규칙 |
|-----|-----------|------|
| `#tag-name` | Brief description | 상위/도메인/세부/기법/유형 |

> **태그 규칙**: <1-line summary of hierarchy rule>

## Weak Areas
- [ ] Area needing review → [[Relevant Note]] → [[Exam Traps]]

## Non-core Topic Policy
| Source | Content | Handling |
|--------|---------|----------|
| <file> | <description> | **Excluded** — reason |
```

## Quick Reference Template

- **Every section heading MUST include `→ [[Concept Note]]` link**
- One-line summary table per concept/term
- Grouped by category
- All key formulas and condition expressions
- "Must-know formulas/patterns" section at bottom with `→ [[Note]]` links

## Exam Traps Template

```markdown
---
keywords: exam traps, weak areas, common mistakes
---

# Exam Traps (시험 함정 포인트)

#dashboard #exam-traps

> [!warning] 이 노트의 목적
> 시험에서 자주 틀리거나 헷갈리는 포인트만 모은 **오답/함정 노트**입니다.

## <Topic 1>

> [!danger]- Trap: <Short description>
> - <What the trap is>
> - <Why it's confusing>
> - <The correct answer/approach>
> - [[Related Concept Note]]

---

## Related
- [[MOC - <Subject>]] → Weak Areas 섹션
- [[빠른 참조]]
```

## Learning Path Template

```markdown
---
keywords: learning path, objectives, prerequisites
---

# Learning Path

#dashboard #learning-path

## Objectives
| Section | By the end you can … |
|---------|----------------------|
| [[01-Topic1]] | explain X; choose between Y and Z |

## Prerequisites
| Learn first | Before | Source |
|-------------|--------|--------|
| [[Concept A]] | [[Concept B]] | source.pdf p.12 |

## Study Order
1. [[Concept A]]
2. [[Concept B]]
```

### Learning Path Rules

- Every prerequisite edge cites the source passage that supports it — drop uncited edges
- No cycles; Study Order is a topological sort of the edges
- Objectives use action verbs (explain, compare, choose, debug) — not "understand X"

## Concept Note Template

```markdown
---
source_pdf: <filename.pdf — MUST match verified Phase 1 mapping>
part: <part number>
keywords: <3-5 English keywords>
---

# <Title> (<Importance: ★~★★★>)

#<tag-from-registry> #<tag-from-registry>

## Overview Table (한눈에 비교)
| Item | Key Point |
|------|-----------|
| A    | ...       |

## <Concept 1>
Concise explanation (3-5 lines max).
- Bullet points for key facts
- Use **bold** for critical terms

---

## Exam/Test Patterns (시험 빈출 패턴)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "keyword X" | **Solution Y** |

## Related Notes
- [[Other Note 1]]
```

### Formatting Rules

- `[[wiki-links]]` for cross-references
- `> [!tip]`, `> [!important]`, `> [!warning]` callouts
- Comparison tables over prose; bold for key vocabulary

### Visualization Rule

Include ASCII diagrams when applicable:
- Processes/stages → timeline or sequence diagram
- Signal/data flow → flow DAG
- Strategy comparisons → quantitative table
- State-based behavior → state transition diagram

### Simplification-with-Exceptions Rule

General statements must check for edge cases — add `> [!warning]` or link to exception details.

## Practice Question Template

```markdown
---
source_pdf: <filename.pdf — MUST match verified Phase 1 mapping>
part: <part number>
keywords: practice, <topic keywords>
---

# <Topic> Practice (N questions)

#practice #<topic-tag>

## Related Concepts
- [[Concept Note 1]]

> [!hint]- 핵심 패턴 (클릭하여 보기)
> | Keyword | Answer |
> |---------|--------|
> | pattern 1 | **Solution** |

---

## Question 1 - <Short Label> [recall]
> Scenario summary in one line

> [!answer]- 정답 보기
> Answer text here with explanation.

---

## Question 2 - <Short Label> [application]
> Given this scenario, what would you do?

> [!answer]- 정답 보기
> Answer with applied reasoning.

---

## Question 3 - <Short Label> [analysis]
> Compare X and Y in this context. Which is better and why?

> [!answer]- 정답 보기
> Comparative analysis answer.

---

> [!summary]- 패턴 요약 (클릭하여 보기)
> | Keyword | Answer |
> |---------|--------|
> | ... | ... |
```

### Practice Question Rules

- Every topic folder MUST have a practice file (8+ questions)
- **Answer hiding**: ALL answers use `> [!answer]- 정답 보기` fold callout
- **Patterns**: `> [!hint]-` / `> [!summary]-` fold callouts (MANDATORY)
- **Question type diversity**: tag `[recall]`, `[application]`, `[analysis]` in heading
  - ≥60% recall, ≥20% application, ≥2 analysis per file
- Scenario in one `>` blockquote line; answer 1-3 lines in fold
- `## Related Concepts` with `[[wiki-links]]` (MANDATORY)
