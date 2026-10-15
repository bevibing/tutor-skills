# Quality Checklist — Self-Review

Before reporting completion, verify every item in the relevant mode's section. Fix and re-verify if any check fails.

---

## Document Mode

### Source Traceability
- [ ] Every source file's content verified (not filename-based assumption)
- [ ] Source content mapping table built and verified in Phase D1
- [ ] Every `source_pdf` frontmatter matches verified mapping
- [ ] Non-academic files excluded and documented
- [ ] Missing sources marked as `원문 미보유`
- [ ] Non-core topic policy documented in MOC

### Coverage
- [ ] Every topic from Phase D2 checklist has a concept note
- [ ] Every enumerated category member has its own note
- [ ] No source topic missing or underrepresented

### Tags
- [ ] All tags: English kebab-case, from registry only
- [ ] Tag Index includes hierarchy rules
- [ ] Detail tags co-attached with parent domain tags

### Structure & Formatting
- [ ] Every note has YAML frontmatter: `source_pdf`, `part`, `keywords`
- [ ] Every concept note has comparison table + exam/test patterns section
- [ ] Process/flow topics have ASCII diagrams
- [ ] Notes are concise (tables > prose)
- [ ] Simplified statements include exception caveats

### Dashboard
- [ ] MOC: Topic Map + Practice Notes + Study Tools + Tag Index + Weak Areas + Non-core Policy
- [ ] MOC links to every concept note AND practice note
- [ ] Weak Areas link to `→ [[note]]` AND `→ [[Exam Traps]]`
- [ ] Exam Traps exists with per-topic fold callouts and bidirectional links
- [ ] Learning Path exists: objectives for every section, cited prerequisite edges, no cycles

### Quick Reference
- [ ] All key formulas and condition expressions included
- [ ] Every section links to concept note via `→ [[Note]]`

### Practice — Active Recall
- [ ] Every topic folder has practice file (8+ questions)
- [ ] All answers use `> [!answer]- 정답 보기` fold — never immediately visible
- [ ] Key Patterns: `> [!hint]-` fold; Pattern Summary: `> [!summary]-` fold
- [ ] `## Related Concepts` with backlinks in every practice file
- [ ] Question type diversity: ≥60% recall, ≥20% application, ≥2 analysis per file

### Interlinking
- [ ] Every concept note has `## Related Notes`
- [ ] `[[wiki-links]]` for all cross-references
- [ ] Siblings reference each other; concept ↔ practice cross-linked
- [ ] Exam Traps ↔ Concept notes bidirectionally linked

### CWD Boundary
- [ ] No source files accessed outside CWD
- [ ] No absolute file paths in notes or frontmatter
- [ ] External URLs accessed only via WebFetch, not file paths

---

## Codebase Mode

### Project Coverage
- [ ] All major modules/domains identified and documented
- [ ] Architecture pattern documented with ASCII diagram
- [ ] Request flow traced end-to-end
- [ ] Data flow documented (input → processing → persistence → output)
- [ ] External dependencies and integrations listed

### Module Completeness
- [ ] Every module has a dedicated note with YAML frontmatter (`module`, `path`, `keywords`)
- [ ] Every module note includes: Purpose, Key Files, Public Interface, Internal Flow, Dependencies
- [ ] Configuration section lists relevant env vars / config keys
- [ ] Testing section includes commands and patterns

### Tags
- [ ] All tags: English kebab-case, from registry only
- [ ] Tag Index in MOC with hierarchy rules
- [ ] Tags cover: `#arch-*`, `#module-*`, `#pattern-*`, `#api-*`

### Dashboard
- [ ] MOC: Architecture Overview + Module Map + API Surface + Getting Started + Tag Index + Onboarding Path
- [ ] MOC links to every module note and exercise file
- [ ] Quick Reference: key commands, env setup, file locations, debugging tips
- [ ] Getting Started section is actionable (copy-paste commands)

### Onboarding Exercises
- [ ] Minimum 5 exercises per major module
- [ ] Exercise types: code reading (trace), configuration, debugging, extension
- [ ] All answers use `> [!answer]- 정답 보기` fold callout
- [ ] Exercises reference relevant module notes via `[[wiki-links]]`

### Interlinking
- [ ] Every module note has `## Related Notes`
- [ ] `[[wiki-links]]` for all cross-references
- [ ] Dependent modules cross-linked bidirectionally
- [ ] Architecture notes reference specific module implementations
- [ ] Exercises link back to the modules they cover

### CWD Boundary
- [ ] No references to files outside the project directory
- [ ] All file paths in notes are relative to project root
- [ ] No hardcoded absolute paths
