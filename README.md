# ResumeHonest: AI Resume Tailor That Won't Lie For You

A Claude skill that tailors resumes to specific job descriptions using deep research, experience discovery, confidence-scored matching, and ATS-safe formatting — while enforcing strict truthfulness guardrails so nothing gets fabricated.

> **Your shot at a job should come down to what you've actually done — not how good you are at writing resumes.** This skill closes that gap without making things up.

---

## What Makes This Different

Most AI resume tools do one of two things wrong: they either keyword-stuff your resume with skills you don't have, or they produce text that's obviously AI-generated. ResumeHonest does neither.

**It researches the role deeply** — not just the JD, but the company culture, what successful people in similar roles look like, and what terminology the hiring team actually uses.

**It finds experience you forgot to document.** A structured branching interview surfaces projects, side work, and transferable skills that aren't in your existing resumes but are relevant to the job.

**It scores every match transparently.** Each bullet in your final resume has a confidence score (direct match / transferable / adjacent / gap) so you know exactly how strong each claim is.

**It refuses to fabricate.** No invented metrics. No inflated titles. No scope exaggeration. Every number in the final resume came from you, not from the AI. Guardrails are enforced at every phase — see [truthfulness-guardrails.md](truthfulness-guardrails.md) for the full rules.

**It strips out AI-sounding language.** The humanisation pass catches "proven track record", "passionate about", "pivotal role", and dozens of other AI tells before they reach your final document.

**It passes ATS parsing.** Single-column layout, standard section headers, proper bullet formatting, keyword-optimised skills sections — all the formatting that Workday, Greenhouse, and Lever need to parse your resume correctly. See [ats-compliance.md](ats-compliance.md).

**It works with whatever you have.** Upload your resume as PDF, DOCX, markdown, or plain text — any combination. No need to convert your files into a specific format first.

---

## How It Works

```
You provide                             You get back
─────────────                           ────────────
Job description (text or URL)      →    Tailored resume (.md + .docx)
Your existing resumes              →    Generation report with match scores
  (.pdf, .docx, .md, or .txt)     →    Gap analysis + interview prep notes
                                   →    Updated experience library
```

### The Workflow

1. **Library build** — Parses your existing resumes into a searchable experience database
2. **Role research** — Analyses the company, role benchmarks, and builds a success profile
3. **Template generation** — Creates an optimised resume structure (you approve before proceeding)
4. **Experience discovery** *(optional)* — Branching interview to surface undocumented skills
5. **Content matching** — Scores and ranks experience against each template slot with confidence levels
6. **Generation** — Produces humanised, ATS-compliant resume in multiple formats
7. **Library update** — Saves successful resumes for future tailoring sessions

Each phase has a user checkpoint. You approve the structure, the content mapping, and the final output before anything is finalised.

### Multi-Job Batch Processing

Applying to 3-5 similar roles? The batch workflow runs experience discovery once across all jobs, then tailors individually — same depth, less repetition.

---

## Quick Start

### Requirements

- **Claude** with skills support (Claude Code or Claude.ai with computer use)
- At least **one existing resume** in any common format (PDF, DOCX, markdown, or plain text)
- A **job description** to tailor against
- The **docx skill** (for Word document generation)
- The **pdf skill** (for reading PDF resumes and optional PDF output)
- Optionally: the **humanizer skill** (for deeper AI-language cleanup)

### Usage

1. Upload your resume file(s) — PDF, DOCX, markdown, or plain text — and the job description
2. Tell Claude: *"Tailor my resume for this job"*
3. Follow the guided workflow — approve structure, review matches, get your files

---

## File Structure

```
resumehonest/
├── SKILL.md                      # Core workflow (339 lines)
├── truthfulness-guardrails.md    # Anti-hallucination rules, reframing constraints
├── ats-compliance.md             # ATS formatting requirements, DOCX rules
├── research-prompts.md           # Company/role research templates
├── matching-strategies.md        # Confidence scoring formula, reframing strategies
├── branching-questions.md        # Experience discovery conversation patterns
├── multi-job-workflow.md         # Batch processing for multiple applications
├── LICENSE                       # MIT
└── README.md
```

### What Each File Does

| File | Purpose | When it's read |
|------|---------|---------------|
| `SKILL.md` | Core workflow orchestration — phases, checkpoints, error handling | Always (entry point) |
| `truthfulness-guardrails.md` | Rules against fabricating metrics, inflating titles, inventing experience | Every phase |
| `ats-compliance.md` | ATS-safe formatting: layout, fonts, headers, keyword strategy | During generation |
| `research-prompts.md` | Templates for JD parsing, company research, role benchmarking | During research phase |
| `matching-strategies.md` | Weighted scoring formula, confidence bands, reframing strategies | During assembly phase |
| `branching-questions.md` | Conversation patterns for surfacing undocumented experience | During discovery phase |
| `multi-job-workflow.md` | Batch processing: shared discovery → per-job tailoring | When multiple JDs provided |

---

## Key Design Decisions

### Why Truthfulness Guardrails Exist

AI resume tools that add fake metrics or inflate scope actively harm job seekers. If you claim "reduced costs by 40%" in an interview and can't explain how you measured that, the interview is over. ResumeHonest treats this as a safety issue: every bullet must be defensible, every metric must come from the user, every title reframing must be disclosed and approved.

### Why ATS Compliance Is Built In

Roughly 75% of resumes are rejected by ATS before a human sees them — often because of formatting issues, not content. A beautiful two-column PDF from Canva might score 0% in Workday because the parser can't read it. ResumeHonest generates ATS-safe DOCX by default: single-column, standard headers, proper bullets, parseable dates.

### Why Experience Discovery Matters

Most people undersell themselves on resumes. Not because they lack experience, but because they forget to document it. The branching interview in this skill systematically surfaces side projects, volunteer work, cross-functional contributions, and transferable skills that never made it into an existing resume. In testing, this phase typically improves JD coverage by 10-20%.

### Why Humanisation Is a First-Class Feature

ATS gets your resume past the software. Humanisation gets it past the human. Recruiters can spot AI-generated resumes instantly — the "proven track record of delivering results in fast-paced environments" opener is a red flag. The humanisation pass strips these patterns so your resume reads like you wrote it.

---

## Example Scenarios

**Internal promotion** — You're applying for a senior role at your current company. The skill researches the team's public work, consolidates your multiple internal roles, and emphasises progression.

**Career transition** — You're moving from engineering to product management. The skill reframes technical leadership as product thinking, surfaces cross-functional work you didn't highlight, and flags genuine gaps for your cover letter.

**Career gap** — You took two years off to start a company. The skill frames this as entrepreneurial experience, surfaces fundraising and product skills, and presents the gap as a strength.

**Batch applications** — You're applying to 4 TPM roles at different companies. The skill runs one discovery session, then tailors each resume individually with company-specific research and terminology.

---

## Frequently Asked Questions

<details>
<summary><strong>Does this work with any LLM or only Claude?</strong></summary>

This is built as a Claude skill and relies on Claude's tool use (file reading, web search, document generation). It's not a standalone app — it runs inside Claude Code or Claude.ai with computer use enabled.
</details>

<details>
<summary><strong>Will it add skills I don't have to match the JD?</strong></summary>

No. The truthfulness guardrails explicitly prohibit adding technologies, skills, or experience the user didn't provide. If there's a gap between your experience and the JD, the skill flags it transparently and gives you options: address it in the cover letter, reframe adjacent experience (with your approval), or accept the gap.
</details>

<details>
<summary><strong>What format does it output?</strong></summary>

Markdown (.md) and Word (.docx) are generated by default. PDF is optional. The DOCX follows ATS-safe formatting rules automatically. A generation report (.md) with match scores and interview prep notes is also included.
</details>

<details>
<summary><strong>What resume formats can I upload?</strong></summary>

PDF, DOCX, markdown (.md), and plain text (.txt) — any combination. Most people upload a PDF of their current resume and that works fine. If you have multiple versions in different formats, upload them all and the skill will parse each one. Note: scanned/image-based PDFs may have limited text extraction — if you have a text-based version, prefer that.
</details>

<details>
<summary><strong>Can I use it for multiple jobs at once?</strong></summary>

Yes. The multi-job workflow handles 3-5 jobs in a single session. It runs experience discovery once, then tailors each resume individually. This saves significant time compared to running the full workflow for each job separately.
</details>

<details>
<summary><strong>What if I only have one resume?</strong></summary>

It works, but with fewer matching options. The skill warns you about a limited library and leans more heavily on the experience discovery phase to surface content that isn't in your existing resume. Your resume can be in any format — PDF, DOCX, markdown, or plain text.
</details>

<details>
<summary><strong>Does it work across sessions?</strong></summary>

Within a single session, everything persists. Across sessions, Claude's file system resets — you'll need to re-upload your resume library and any previously generated files. The skill generates portable output files you can download and re-upload later.
</details>

---

## Contributing

Contributions welcome. The areas where help would be most valuable:

- **Testing across job types** — The skill has been tested primarily on PM/TPM/engineering roles. Testing with creative, healthcare, legal, or academic resumes would help broaden coverage.
- **ATS parser validation** — If you have access to specific ATS systems, testing whether generated DOCX files parse correctly would be extremely useful.
- **Humanisation patterns** — New AI-writing tells emerge constantly. Additions to the humanisation checklist are valuable.
- **Experience discovery patterns** — New branching question patterns for specific skill categories.

## Author

**Siv Souvam** — [GitHub](https://github.com/sivsouvam) · [LinkedIn](https://linkedin.com/in/sivsouvam)

## License

MIT — see [LICENSE](LICENSE)

---

<sub>Built as a Claude skill. Not affiliated with Anthropic. "Claude" is a trademark of Anthropic, PBC.</sub>
