---
name: resumehonest
description: |
  Use when creating tailored, human-sounding resumes for job applications.
  Accepts resumes in any common format (PDF, DOCX, markdown, plain text).
  Goes beyond keyword matching — researches the company and role deeply,
  runs a structured experience discovery conversation to surface skills the
  user hasn't documented yet, scores content matches with confidence levels,
  and generates polished multi-format resumes that read like a real person
  wrote them (not an AI). Combines resume tailoring with automatic humanisation
  to strip out AI-sounding language. Use whenever someone wants to apply for
  a job, optimise their resume for a specific role, or build a resume library.
  Works for single applications and batch processing across multiple jobs.
  Also use when user says "tailor my resume", "optimise for this job",
  "help me apply", or provides a JD and asks for resume help.
---

# Tailored & Humanised Resume Skill

## Overview

Builds job-winning resumes that are both strategically optimised for a specific role *and* genuinely human in tone. Most AI-generated resumes fail not because the content is wrong, but because they sound like a machine wrote them. This skill fixes both problems together.

Two tracks run simultaneously:
- **Relevance track:** Surface the right experiences, match them to what the role needs, structure the story intelligently
- **Humanisation track:** Strip out AI tells at every stage so the final document reads like the applicant wrote it themselves

**Guiding principle:** A person's shot at a job should come down to their actual experience, not how polished their resume writing is. This skill closes that gap without fabricating anything.

**Author:** Siv Souvam

## Reference Files

Read these files as needed during each phase:

| File | Read during | Purpose |
|------|-------------|---------|
| `research-prompts.md` | Phase 1 | Company/role research templates, success profile synthesis |
| `matching-strategies.md` | Phase 3 | Confidence scoring formula, reframing strategies |
| `branching-questions.md` | Phase 2.5 | Experience discovery conversation patterns |
| `truthfulness-guardrails.md` | **All phases** | Anti-hallucination rules, title reframing constraints, metric verification |
| `ats-compliance.md` | Phase 4 | ATS formatting rules, DOCX structure requirements |
| `multi-job-workflow.md` | When multi-job detected | Batch processing workflow |

**CRITICAL:** Always read `truthfulness-guardrails.md` before starting any resume generation. Its rules apply to every phase.

## When to Use

Use this skill when:
- User has a job description and wants a resume tailored to it
- User has existing resumes to draw from (PDF, DOCX, markdown, or plain text)
- User wants to optimise their application for a specific role or company
- User needs help finding and articulating experiences they haven't documented yet
- User wants their resume to sound natural, not AI-generated

**Not designed for:**
- Writing a resume from scratch with no existing material (needs at least one source resume in any format)
- Cover letters or LinkedIn optimisation (separate workflows)

## Quick Start

**Required from user:**
1. Job description (text or URL)
2. One or more existing resumes (PDF, DOCX, markdown, or plain text — any combination)
3. Resume location: uploaded files in conversation, or a directory path

**Workflow:**
1. Build library from existing resumes → Phase 0
2. Research company/role → Phase 1
3. Create template (with user checkpoint) → Phase 2
4. Optional: Branching experience discovery → Phase 2.5
5. Match content with confidence scoring → Phase 3
6. Generate MD + DOCX + Report → Phase 4
7. User review → Optional library update → Phase 5

## Multi-Job Detection

Before starting single-job workflow, check for multi-job signals:
- Multiple JD URLs
- Phrases like "multiple jobs", "several positions", "batch"
- Multiple company/role mentions

If detected, ask user if they want multi-job mode. If yes, read `multi-job-workflow.md` and follow that workflow instead. Single-job workflow below is unchanged.

---

## Phase 0: Library Initialization

**Goal:** Build fresh experience database from user's existing resumes, regardless of format.

**Supported input formats:** PDF, DOCX, Markdown (.md), Plain text (.txt)

**Process:**

1. **Locate resumes:**
   - If user uploaded files directly: use uploaded files from `/mnt/user-data/uploads/`
   - If user provides a directory path: scan that directory
   - Accept any mix of PDF, DOCX, MD, and TXT files

2. **Parse each resume by format:**
   - **PDF:** Use the pdf skill to extract text. If the PDF is image-based (scanned), attempt OCR. Warn user if extraction quality is low.
   - **DOCX:** Use the docx skill or python-docx to extract text content, preserving section structure where possible.
   - **Markdown:** Read directly — section headers map to roles, bullets map to achievements.
   - **Plain text:** Read directly — use heuristics (date patterns, company names, indentation) to identify sections.

3. **Normalise into common structure:** Regardless of source format, extract into the same in-memory database:

```json
{
  "roles": [{
    "role_id": "company_title_year",
    "company": "Company Name",
    "title": "Job Title",
    "dates": "YYYY-YYYY",
    "bullets": [{
      "text": "Full bullet text",
      "themes": ["leadership", "technical"],
      "metrics": ["17x improvement", "$3M revenue"],
      "keywords": ["cross-functional", "program"],
      "source_file": "resume.pdf",
      "source_format": "pdf"
    }]
  }],
  "skills": { "technical": [], "product": [], "leadership": [] },
  "education": []
}
```

4. **Auto-tag content:** themes, metrics (extract numbers/percentages/dollars), keywords
5. **Announce:** "Built library from {N} resumes ({X} PDF, {Y} DOCX, {Z} other) — {A} roles, {B} unique bullets"

**Format-specific gotchas:**
- **PDF:** Multi-column PDFs may extract with jumbled text order. If the parsed text looks garbled, warn the user and ask if they have a DOCX or text version.
- **DOCX:** Tables used for layout (common in designed resumes) may parse as fragmented text. Flag if structure looks wrong.
- **Scanned PDFs:** OCR quality varies. If text extraction yields very little content, tell the user: "This looks like a scanned PDF. I got limited text from it. Do you have a text-based version?"

**Output:** In-memory database ready for matching, with source file and format tracked per bullet.

---

## Phase 1: Research

**Goal:** Build "success profile" — what actually makes someone good at this job, beyond the JD's literal text.

Read `research-prompts.md` for full templates. High-level:

1. **JD Parsing:** Extract explicit requirements (must-have vs nice-to-have), technical keywords, implicit preferences, red flags, role archetype
2. **Company Research:** WebSearch for mission/values/culture, engineering blog, recent news
3. **Role Benchmarking:** Search LinkedIn for similar role holders, analyze common backgrounds and terminology
4. **Success Profile Synthesis:** Combine into structured profile with core requirements, valued capabilities, cultural fit signals, terminology map, risk factors

**Checkpoint:** Present success profile summary to user. Wait for confirmation before proceeding.

**Fallback:** If research is limited (obscure company, WebSearch fails), fall back to JD-only analysis and ask user for context.

---

## Phase 2: Template Generation

**Goal:** Create resume structure optimised for this specific role.

**Process:**

**2.1 Analyse library:** Extract role archetypes, experience clusters, career progression from user's existing resumes.

**2.2 Role consolidation decision:** When user held multiple roles at same company, decide whether to consolidate or keep separate. Present both options with rationale. Different companies are ALWAYS separate.

**2.3 Title reframing:** Adjust titles to emphasize aspects most relevant to target role.

⚠️ **CRITICAL — Read `truthfulness-guardrails.md` for title reframing rules.** Key constraints:
- NEVER claim work you didn't do
- NEVER inflate seniority beyond defensible
- Company name and dates MUST be exact
- Always show original title alongside proposed reframing
- Flag any reframing where new title implies a different job level

**2.4 Generate template skeleton:**

```markdown
## Professional Summary
[GUIDANCE: {X} sentences emphasizing {themes from success profile}]

## Key Skills
[STRUCTURE: {2-4 categories based on JD structure}]

## Professional Experience
### [ROLE 1 - Most Recent/Relevant]
[TITLE OPTIONS: A: {option}, B: {option}, Recommended: {choice}]
[BULLET ALLOCATION: {N} bullets]
Bullet 1: [SEEKING: {requirement type}]
Bullet 2: [SEEKING: {requirement type}]

### [ROLE 2]
...

## Education
```

**Checkpoint:** Present template to user showing structure, consolidation decisions, title options, and bullet allocation. Wait for approval.

---

## Phase 2.5: Experience Discovery (OPTIONAL)

**Goal:** Surface undocumented experiences through conversational branching interview.

**Trigger:** After template approval, if gaps identified (confidence < 60%):

```
"I've identified {N} gaps or weak matches:
- {Gap 1}: {Current confidence}%
- {Gap 2}: {Current confidence}%

Would you like a structured brainstorming session? (10-15 minutes typical)"
```

Read `branching-questions.md` for full conversation patterns.

**Core approach:** For each gap, start with open probe → branch based on answer (direct experience / indirect / adjacent / personal / no) → follow up systematically → capture immediately.

**Capture structure per experience:**
- Context (where/when), Scope (scale, duration, impact)
- Which gaps it addresses
- Draft bullet
- Confidence improvement estimate

**After discovery:** For each captured experience, user decides: Add to current resume / Add to library only / Refine further / Discard.

⚠️ **CRITICAL:** During discovery, help the user articulate what they did — but NEVER fabricate details, metrics, or specifics they didn't provide. See `truthfulness-guardrails.md`.

---

## Phase 3: Assembly

**Goal:** Fill approved template with best-matching content, with transparent scoring.

Read `matching-strategies.md` for the full scoring system.

**For each template slot:**

1. **Score candidates** from library using weighted formula:
   - Direct match (40%): Keywords, domain, technology, outcome overlap
   - Transferable (30%): Same capability in different context
   - Adjacent (20%): Related tools, methods, problem space
   - Impact (10%): Achievement type alignment
   - Overall = (Direct × 0.4) + (Transfer × 0.3) + (Adjacent × 0.2) + (Impact × 0.1)

2. **Present top 3 matches** with confidence bands:
   - 90-100%: DIRECT — use with confidence
   - 75-89%: TRANSFERABLE — strong candidate
   - 60-74%: ADJACENT — acceptable with reframing
   - <60%: WEAK/GAP — flag to user

3. **Handle gaps** (confidence < 60%):
   - Option 1: Reframe adjacent experience (show before/after with truthfulness justification)
   - Option 2: Flag for cover letter
   - Option 3: Omit bullet slot
   - Option 4: Use best available with disclosure
   - User decides.

4. **Apply content reframing** where needed (see `matching-strategies.md`):
   - Keyword alignment, emphasis shift, abstraction level, scale emphasis
   - Always show original → reframed with explanation

**Checkpoint:** Present complete mapping with coverage summary, reframings applied, gaps identified, overall JD coverage %. Wait for user approval.

⚠️ **CRITICAL:** Every reframed bullet must pass the truthfulness test in `truthfulness-guardrails.md`. Never add metrics, facts, or specifics not provided by the user.

---

## Phase 4: Generation

**Goal:** Create professional multi-format outputs.

**4.0 Humanisation Pass:**

Before compiling final outputs, run humanisation on all generated text. Apply the humanizer skill patterns. Focus on resume-specific AI tells:

- **Promotional language:** "passionate about", "results-driven", "proven track record" → Replace with specific facts
- **Significance inflation:** "pivotal role", "key contributor" → Concrete verbs and numbers
- **Vague attribution:** "led efforts to improve X" → "Reduced X by Y% by doing Z"
- **Rule-of-three padding:** "Built, deployed, and optimised X while fostering collaboration" → Break apart or cut
- **Sycophantic summary openers:** "Highly motivated professional" → Direct statement of what they do

**Concreteness test:** If you can remove a phrase and the bullet still means the same thing, remove it. If a phrase could appear on anyone's resume, replace it with something specific.

**4.1 Markdown generation:** Compile mapped content into clean markdown using user's formatting preferences.

**4.2 DOCX generation:** Use the docx skill. Read `ats-compliance.md` for ATS-safe formatting requirements.

**4.3 PDF generation (optional):** Use the pdf skill if user requests PDF.

**4.4 Generation summary report:** Create metadata report with target role summary, content mapping summary, reframings applied, source resumes used, gaps addressed, key differentiators, and interview prep recommendations.

**Present to user:** List files created, quality metrics (JD coverage %, direct matches %, newly discovered experiences), and ask for review decision.

---

## Phase 5: Library Update (CONDITIONAL)

After user reviews generated resume:

**Option 1 — Save to library:** Move files to library directory, rebuild database, preserve generation metadata (source resumes, reframings, match scores, discovered experiences).

**Option 2 — Need revisions:** Collect feedback, make changes, re-present.

**Option 3 — Save but don't add to library:** Keep files in current location, don't enrich database.

---

## Error Handling

**Insufficient library (1-2 resumes):** Warn about limited matching options, emphasize discovery phase value, proceed with available content.

**No good matches (<60% for critical requirement):** Present options transparently — run discovery, reframe best available, omit slot, note for cover letter. Never force matches.

**Research failures:** Fall back to JD-only analysis, ask user for context about company culture/tech/structure.

**Vague JD:** Flag missing detail areas, ask user for additional context, work with what's available.

**Resume too long:** Present pruning suggestions ranked by relevance score, let user decide priority.

**Generation failures:** DOCX/PDF fail → fall back to markdown-only with error details.

**General principles:**
- All checkpoints allow going back to previous phase
- Progress saved between phases where possible
- Transparent about limitations at every step
- User always has final decision authority

---

## Usage Examples

**Example 1: Internal role (same company)**
- Library build → 29 resumes found
- Research → Internal culture + role benchmarking
- Discovery → Surfaces side projects and undocumented work
- Assembly → 92% JD coverage, 75% direct matches
- Result: Highly competitive application leveraging internal experience

**Example 2: Career transition**
- Research → Cross-domain transfer patterns identified
- Template → Reframes titles to emphasize transferable aspects
- Discovery → Surfaces volunteer work and graduate research in target domain
- Assembly → 65% coverage, gaps flagged for cover letter
- Result: Bridges technical skills with new domain

**Example 3: Career gap**
- Template → Includes startup period as legitimate role
- Discovery → Surfaces entrepreneurial skills (fundraising, product dev, team building)
- Assembly → Gap becomes strength showing initiative
- Result: Gap framed as valuable experience

**Example 4: Multi-job batch (3 similar roles)**
- See `multi-job-workflow.md` for complete batch processing workflow
- Shared discovery → per-job tailoring
- 3 resumes in ~40 minutes vs ~45 minutes sequential
