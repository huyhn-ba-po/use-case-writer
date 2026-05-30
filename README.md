# Use Case Writer

> A Claude AI skill that helps IT Business Analysts **scope, analyze, and document Use Cases** in English Markdown, following the industry-standard 13-field template (Karl Wiegers / IIBA style) with best practices from Alistair Cockburn's *Writing Effective Use Cases*.

[![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange)](https://claude.ai)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)
[![Output](https://img.shields.io/badge/Output-English%20Markdown-green)](#)
[![Template](https://img.shields.io/badge/Template-Karl%20Wiegers%20%2F%20IIBA-blue)](#)

The examples in this skill use neutral, general-purpose domains (e-commerce, facility booking, SaaS) — they illustrate the method, not any specific product or industry.

---

## Why this skill?

Writing a high-quality Use Case spec is harder than it looks. BAs commonly struggle with:

- **Wrong scope**: writing UCs that are too big (entire workflows) or too small (sub-functions like "Verify OTP")
- **Vague actors**: using "User" instead of specific roles like Customer, Member, or Facility Manager
- **Embedded logic**: stuffing if/else and loops into the Normal Course
- **Missing failure modes**: only writing the happy path
- **Confusing Preconditions with Business Rules**

This skill enforces the discipline by:

1. **Scoping first** — applying Cockburn's coffee-break test, goal levels, and system boundary rules before writing anything
2. **Generating sequentially** — section by section, stopping for confirmation so issues are caught early
3. **Validating with a 20-point checklist** — every UC is reviewed before handover

---

## Features

- ✅ **English Markdown output** following the standard 13-field template
- ✅ **Sequential workflow** — 5 section groups, with confirmation gates between them
- ✅ **4 modes**: write new, split feature into UC list, refine existing, or write a specific section
- ✅ **Scoping rules** based on Alistair Cockburn (coffee-break test, goal levels, one-actor-one-goal-one-session, system boundary)
- ✅ **3 identification techniques** for breaking down large features: goal-driven, event-driven, CRUD-driven
- ✅ **20-point quality checklist** auto-run before handover
- ✅ **Bilingual interaction** — chat in Vietnamese or English, produce the UC artifact in English
- ✅ **2 complete UC examples** included as reference (Place an Order, Approve a Booking Request)

---

## Repository structure

```
use-case-writer/
├── SKILL.md                          # Main workflow (loaded into Claude's context)
├── references/                       # Reference docs (loaded on demand)
│   ├── template-guide.md             # How to fill each of the 13 fields, with worked examples
│   ├── writing-style.md              # Active voice rules, numbering conventions, 10 anti-patterns
│   ├── quality-checklist.md          # 20-point validation checklist with pass/fail examples
│   └── examples.md                   # 2 full UC examples (Place an Order, Approve a Booking Request)
├── assets/
│   └── uc-template.md                # Copy-ready Markdown template
├── README.md                         # This file
├── LICENSE                           # MIT
├── CHANGELOG.md                      # Version history
└── .gitignore
```

The skill follows the **progressive disclosure** pattern: only `SKILL.md` is always loaded into Claude's context. The `references/` and `assets/` files are loaded only when needed, keeping context usage efficient.

---

## Installation

### Option 1: Upload to a Claude Project (recommended)

1. Download or clone this repository
2. In [claude.ai](https://claude.ai), go to **Projects → Create new project**
3. Upload the entire `use-case-writer/` folder to the project knowledge base
4. Start chatting — the skill activates automatically on relevant prompts

### Option 2: Use as a prompt template

Paste the contents of `SKILL.md` at the start of a chat and tell Claude:
> "Follow this workflow whenever I ask you to write a use case."

### Option 3: Deploy to /mnt/skills/user/

If your environment supports the skills mount:
1. Copy the folder to `/mnt/skills/user/use-case-writer/`
2. Claude will detect and trigger the skill automatically

---

## Usage

### Trigger phrases

**English**:
- "Write a use case for [feature]"
- "Draft a UC for [functionality]"
- "Split this feature into a UC list"
- "Review my UC"
- "Write the Normal Course for this UC"

**Vietnamese** (skill responds in Vietnamese, UC artifact in English):
- "Viết use case cho [tính năng]"
- "Viết UC cho chức năng này"
- "Tách feature này thành UC list"
- "Review UC này giúp tôi"
- "Đặc tả use case cho [tính năng]"

### Example interaction

```
User: Viết UC cho tính năng đặt phòng họp

Claude:
[Step 1] Mode: A (write new from feature description).
[Step 2] Xác nhận scope:
  Primary actor: Member (workspace plan subscriber)
  Goal: Book a meeting room
  System boundary: Workspace platform

Scope confirmed ✅. Confirm để tiến hành Step 3 (sinh UC theo từng nhóm)?

User: yes

Claude:
[Group 1: Identification + Actor + Description]

| Use Case ID: | UC-ROOM-01 |
| Use Case Name: | Book a meeting room |
| ...

Group 1 done. Confirm to proceed to preconditions, postconditions, priority, frequency?

[... continues section by section ...]
```

### The 4 workflow steps

```
Step 1: CLASSIFY INPUT     →  identify which of the 4 modes the user is in
Step 2: SCOPE THE UC       →  apply 4 scoping rules + coffee-break test
Step 3: WRITE THE UC       →  fill 13 fields, ONE SECTION GROUP AT A TIME
Step 4: VALIDATE           →  run 20-point checklist before handover
```

---

## The 13 fields covered

| Field | Purpose |
|-------|---------|
| Use Case ID | Unique identifier (e.g. UC-ORDER-01) |
| Use Case Name | Action verb + Object |
| History | Created By / Date, Last Updated By / Date |
| Actor | Primary + Secondary actors (specific roles) |
| Description | 2-3 sentences: WHY + WHAT + OUTCOME |
| Preconditions | Verifiable conditions before UC starts |
| Postconditions | System state after successful completion |
| Priority | High / Medium / Low with justification |
| Frequency of Use | Quantified (X times / time unit) |
| Normal Course | Numbered, alternating Actor / System steps |
| Alternative Courses | Different paths to success (AC.1, AC.2…) |
| Exceptions | Failure modes (EX.1, EX.2…) |
| Includes | Sub-UCs called for common functionality |
| Special Requirements | Non-functional requirements |
| Assumptions | Beliefs not yet verified |
| Notes and Issues | TBDs with owner + due date |

---

## The 20-point quality checklist

Every UC is validated before handover:

| Group | Items | What it checks |
|-------|-------|----------------|
| **A. Scope & Identification** | C1-C5 | Name format, goal level, ID uniqueness, single primary actor, system boundary |
| **B. Actor & Context** | C6-C8 | Specific actor role, complete description, quantified frequency |
| **C. Pre/Post Conditions** | C9-C11 | Verifiable preconditions, complete postconditions, no Pre/Assumption mix |
| **D. Normal Course** | C12-C15 | Numbered steps, Actor/System alternation, no embedded logic, complete flow |
| **E. Alternative & Exception** | C16-C18 | AC format, exception structure, common failure modes covered |
| **F. Completeness** | C19-C20 | Valid Includes, non-functional Special Requirements |

See [`references/quality-checklist.md`](references/quality-checklist.md) for full details.

---

## What this skill does NOT do

- ❌ **Agile User Stories** — use a dedicated user-story / acceptance-criteria skill for that
- ❌ **Full PRD / URD / SRS documents** — UC is one section, not the whole doc
- ❌ **UML Use Case Diagrams** — this skill produces text specs, not diagrams
- ❌ **Wireframes or UI mockups** — UC describes interaction, not visual design
- ❌ **Business Process Models** — BP covers multi-actor multi-system processes; UC is 1 actor + 1 system

---

## Examples included

Two complete, validated UCs ship with the skill in [`references/examples.md`](references/examples.md):

1. **UC-ORDER-01: Place an order for a physical product**
   - Customer-facing UC with payment integration and async inventory fallback
   - 9-step Normal Course, 2 Alternative Courses, 4 Exceptions
   - Demonstrates: gift card payment AC, out-of-stock race condition, async retry on inventory failure

2. **UC-ROOM-03: Approve a meeting-room booking request**
   - Staff-facing admin UC with concurrency, credit enforcement, and calendar integration
   - 10-step Normal Course, 2 Alternative Courses, 4 Exceptions
   - Demonstrates: concurrency conflict, credit exhaustion at approval time, calendar service degraded-mode handling

---

## References & inspiration

- **Alistair Cockburn**, *Writing Effective Use Cases* (Addison-Wesley, 2000)
- **Karl Wiegers & Joy Beatty**, *Software Requirements* (Microsoft Press, 3rd ed.)
- **IIBA**, *BABOK Guide v3* — Use Case and Scenarios technique
- **Ivar Jacobson**, *Object-Oriented Software Engineering* — origin of use case modeling

---

## Contributing

Contributions welcome! If you have:
- Additional UC examples from your domain (insurance, healthcare, e-commerce, logistics…)
- New anti-patterns you've encountered in the field
- Improvements to the 20-point checklist
- Translations or localization notes

Please open a Pull Request or Issue. When contributing UC examples, ensure they pass the full 20-point checklist first.

---

## License

MIT License — see [LICENSE](LICENSE) for details.
