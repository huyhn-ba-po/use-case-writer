# Changelog · Use Case Writer

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [1.0.0]

### Initial release

#### Added
- Core `SKILL.md` with 4-step workflow (Classify → Scope → Write → Validate)
- 4 operating modes: write new, split into UC list, refine existing, write specific section
- Sequential generation in 5 section groups with confirmation gates
- 20-point quality checklist (Groups A-F covering Scope, Actor, Conditions, Flow, AC/EX, Completeness)
- Alistair Cockburn's scoping rules: coffee-break test, 3 goal levels, system boundary
- 3 UC identification techniques: goal-driven, event-driven, CRUD-driven
- `references/template-guide.md` — field-by-field guidance with worked examples
- `references/writing-style.md` — active voice rules, numbering conventions, 10 anti-patterns
- `references/quality-checklist.md` — full 20-point checklist with pass/fail examples
- `references/examples.md` — 2 complete UC examples:
  - UC-ORDER-01: Place an order for a physical product
  - UC-ROOM-03: Approve a meeting-room booking request
- `assets/uc-template.md` — copy-ready Markdown template

#### Configuration
- Output language: English (artifact always in English regardless of user's chat language)
- Output format: Markdown with 2-column table layout
- Workflow mode: Sequential (section-by-section with confirmation gates)
- Domain examples: neutral, general-purpose (e-commerce, facility booking, SaaS)
