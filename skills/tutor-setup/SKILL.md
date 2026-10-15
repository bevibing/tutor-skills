---
name: tutor-setup
description: >
  Transforms knowledge sources into an Obsidian StudyVault. Two modes:
  (1) Document Mode — PDF/text/web sources → study notes with practice questions.
  (2) Codebase Mode — source code project → onboarding vault for new developers.
  Mode is auto-detected based on project markers in CWD.
argument-hint: "[source-path-or-url]"
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, WebFetch
---

# Tutor Setup — Knowledge to Obsidian StudyVault

## CWD Boundary Rule (ALL MODES)

> **NEVER access files outside the current working directory (CWD).**
> All source scanning, reading, and vault output MUST stay within CWD and its subdirectories.
> If the user provides an external path, ask them to copy the files into CWD first.

## Mode Detection

On invocation, detect mode automatically:

1. **Check for project markers** in CWD:
   - `package.json`, `pom.xml`, `build.gradle`, `Cargo.toml`, `go.mod`, `Makefile`,
     `*.sln`, `pyproject.toml`, `setup.py`, `Gemfile`
2. **If any marker found** → **Codebase Mode**
3. **If no marker found** → **Document Mode**
4. **Tie-break**: If `.git/` is the sole indicator and no source code files (`*.ts`, `*.py`, `*.java`, `*.go`, `*.rs`, etc.) exist, default to Document Mode.
5. Announce detected mode and ask user to confirm or override.

---

## Document Mode

> Transforms knowledge sources (PDF, text, web, epub) into study notes.
> Templates: [templates.md](references/templates.md)

### Phase D1: Source Discovery & Extraction

1. **Auto-scan CWD** for `**/*.pdf`, `**/*.txt`, `**/*.md`, `**/*.html`, `**/*.epub` (exclude `node_modules/`, `.git/`, `dist/`, `build/`, `StudyVault/`). Present for user confirmation.
2. **Extract text (MANDATORY tools)**:
   - **PDF → `pdftotext` CLI ONLY** (run via Bash tool). NEVER use the Read tool directly on PDF files — it renders pages as images and wastes 10-50x more tokens. Convert to `.txt` first, then Read the `.txt` file.
     ```bash
     pdftotext "source.pdf" "/tmp/source.txt"
     ```
   - If `pdftotext` is not installed, install it first: `brew install poppler` (macOS) or `apt-get install poppler-utils` (Linux).
   - URL → WebFetch
   - Other formats (`.md`, `.txt`, `.html`) → Read directly.
3. **Read extracted `.txt` files** — understand scope, structure, depth. Work exclusively from the converted text, never from the raw PDF.
4. **Source Content Mapping (MANDATORY for multi-file sources)**:
   - Read **cover page + TOC + 3+ sample pages from middle/end** for EVERY source file
   - **NEVER assume content from filename** — file numbering often ≠ chapter numbering
   - Build verified mapping: `{ source_file → actual_topics → page_ranges }`
   - Flag non-academic files and missing sources
   - Present mapping to user for verification before proceeding

### Phase D2: Content Analysis

1. Identify topic hierarchy — sections, chapters, domain divisions.
2. Separate concept content vs practice questions.
3. Map dependencies between topics as prerequisite edges ("learn A before B"). Keep only edges the source supports, with a source citation for each.
4. Identify key patterns — comparisons, decision trees, formulas.
5. **Full topic checklist (MANDATORY)** — every topic/subtopic listed. Drives all subsequent phases.

> **Equal Depth Rule**: Even a briefly mentioned subtopic MUST get a full dedicated note supplemented with textbook-level knowledge.

6. **Classification completeness**: When source enumerates categories ("3 types of X"), every member gets a dedicated note. Scan for: "types of", "N가지", "categories", "there are N".
7. **Source-to-note cross-verification (MANDATORY)**: Record which source file(s) and page range(s) cover each topic. Flag untraceable topics as "source not available".

### Phase D3: Tag Standard

Define tag vocabulary before creating notes:
- **Format**: English, lowercase, kebab-case (e.g., `#data-hazard`)
- **Hierarchy**: top-level → domain → detail → technique → note-type
- **Registry**: Only registered tags allowed. Detail tags co-attach parent domain tag.

### Phase D4: Vault Structure

Create `StudyVault/` with numbered folders per [templates.md](references/templates.md). Group 3-5 related concepts per file.

### Phase D5: Dashboard Creation

Create `00-Dashboard/`: MOC, Quick Reference, Exam Traps, Learning Path. See [templates.md](references/templates.md).

- **MOC**: Topic Map + Practice Notes + Study Tools + Tag Index (with rules) + Weak Areas (with links) + Non-core Topic Policy
- **Quick Reference**: every heading includes `→ [[Concept Note]]` link; all key formulas
- **Exam Traps**: per-topic trap points in fold callouts, linked to concept notes
- **Learning Path**: per-section objectives ("By the end you can …") + prerequisite edges from Phase D2, in topological order

### Phase D6: Concept Notes

Per [templates.md](references/templates.md). Key rules:
- YAML frontmatter: `source_pdf`, `part`, `keywords` (MANDATORY)
- **source_pdf MUST match verified Phase D1 mapping** — never guess from filename
- If unavailable: `source_pdf: 원문 미보유`
- `[[wiki-links]]`, callouts (`[!tip]`, `[!important]`, `[!warning]`), comparison tables > prose
- ASCII diagrams for processes/flows/sequences
- **Simplification-with-exceptions**: general statements must note edge cases

### Phase D7: Practice Questions

Per [templates.md](references/templates.md). Key rules:
- Every topic folder MUST have a practice file (8+ questions)
- **Active recall**: answers use `> [!answer]- 정답 보기` fold callout
- Patterns use `> [!hint]-` / `> [!summary]-` fold callouts
- **Question type diversity**: ≥60% recall, ≥20% application, ≥2 analysis per file
- `## Related Concepts` with `[[wiki-links]]`

### Phase D8: Interlinking

1. `## Related Notes` on every concept note
2. MOC links to every concept + practice note
3. Cross-link concept ↔ practice; siblings reference each other
4. Quick Reference sections → `[[Concept Note]]` links
5. Weak Areas → relevant note + Exam Traps; Exam Traps → concept notes

### Phase D9: Self-Review (MANDATORY)

Verify against [quality-checklist.md](references/quality-checklist.md) **Document Mode** section. Fix and re-verify until all checks pass.

---

## Codebase Mode

> Generates a new-developer onboarding StudyVault from a source code project.
> Full workflow: [codebase-workflow.md](references/codebase-workflow.md)
> Templates: [codebase-templates.md](references/codebase-templates.md)

### Phase Summary

| Phase | Name | Key Action |
|-------|------|------------|
| C1 | Project Exploration | Scan files, detect tech stack, read entry points, map directory layout |
| C2 | Architecture Analysis | Identify patterns, trace request flow, map module boundaries and data flow |
| C3 | Tag Standard | Define `#arch-*`, `#module-*`, `#pattern-*`, `#api-*` tag registry |
| C4 | Vault Structure | Create `StudyVault/` with Dashboard, Architecture, per-module, DevOps, Exercises folders |
| C5 | Dashboard | MOC (Module Map + API Surface + Getting Started + Onboarding Path) + Quick Reference |
| C6 | Module Notes | Per-module notes: Purpose, Key Files, Public Interface, Internal Flow, Dependencies |
| C7 | Onboarding Exercises | Code reading, configuration, debugging, extension exercises (5+ per major module) |
| C8 | Interlinking | Cross-link modules, architecture ↔ implementations, exercises ↔ modules |
| C9 | Self-Review | Verify against [quality-checklist.md](references/quality-checklist.md) **Codebase Mode** section |

See [codebase-workflow.md](references/codebase-workflow.md) for detailed per-phase instructions.

---

## Language

- Match source material language (Korean → Korean notes, etc.)
- **Tags/keywords**: ALWAYS English
