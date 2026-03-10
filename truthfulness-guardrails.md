# Truthfulness Guardrails

## Overview

This file contains the non-negotiable rules for maintaining factual integrity throughout resume generation. Read this before starting any resume work. Every phase — research, template generation, experience discovery, assembly, and generation — must comply with these rules.

The core principle: help people present their real experience in the strongest possible light. Never cross the line into fabrication, inflation, or invention.

## Hard Rules (Never Break These)

### 1. Never Invent Metrics

If the user didn't provide a number, don't create one. This is the single most common way AI resume tools damage credibility.

**Forbidden patterns:**
- User says "I improved deployment speed" → You write "Reduced deployment time by 40%"
- User says "I managed a team" → You write "Led team of 12 engineers"
- User says "I increased sales" → You write "Drove $2.3M revenue increase"
- User says "I helped with cost reduction" → You write "Achieved 30% cost savings"

**What to do instead:**
- Ask: "Do you have a rough number for that improvement?"
- If they don't know: Write without the metric — "Improved deployment speed by streamlining the CI/CD pipeline"
- If they give a range: Use the conservative end — "Reduced deployment time by approximately 25%"
- If they say "I think it was around X": Use it with appropriate framing — "~X" or "approximately X"

**The test:** Can the user defend every number in an interview? If you're not sure, ask them.

### 2. Never Fabricate Experience

Don't invent projects, responsibilities, tools, or technologies the user didn't mention.

**Forbidden:**
- Adding technologies the user didn't claim (e.g., adding "Docker" because the JD wants it)
- Inventing project outcomes or business results
- Creating responsibilities that go beyond what the user described
- Combining experiences from different contexts into a single fabricated bullet

**Acceptable:**
- Asking "Did you use Docker as part of this?" (discovery, not invention)
- Reframing existing experience to highlight relevant aspects
- Using terminology from the JD when the user's experience genuinely matches

### 3. Never Inflate Scope or Seniority

Don't make roles sound bigger than they were.

**Forbidden patterns:**
- "Contributed to" → "Spearheaded"
- "Helped the team with" → "Led organization-wide initiative"
- "Worked on a feature" → "Architected the platform"
- "Participated in meetings" → "Drove strategic alignment across functions"
- Adding "senior" or "principal" to titles when not earned
- Implying direct reports when they had none
- Claiming "managed" when they "coordinated" or "supported"

**The test:** Would the user's former manager agree with this characterisation? If there's any doubt, soften it.

### 4. Preserve Company Names and Dates Exactly

No exceptions. These are verifiable facts.

- Company name: Exact legal or commonly used name
- Dates: Exact start and end (month/year)
- Location: Exact city/region
- Never round up tenure ("almost 3 years" stays as the actual months)

## Title Reframing Rules

Title reframing is legitimate when it helps a reader understand what you actually did. It becomes dishonest when it implies a role you didn't hold.

### Acceptable Reframing

**Emphasise a different aspect of the same role:**
- "Graduate Researcher" → "Research Software Engineer" — ONLY if the role was primarily coding
- "Data Science Lead" → "Technical Program Manager" — ONLY if the role was primarily program management, not individual data science work

**Use industry-standard terminology:**
- "Scientist III" → "Senior Research Scientist" — mapping internal levels to recognisable titles
- "Program Coordinator" → "Project Manager" — when responsibilities match PM scope

**Add truthful specialisation:**
- "Engineer" → "ML Engineer" — ONLY if ML work was substantial (not occasional)

### Forbidden Reframing

- Changing the level (IC → Manager, Junior → Senior) without evidence
- Changing the function (QA → Software Engineer, Marketing → Product Manager) unless the work genuinely was that function
- Inventing titles that never existed at the company
- Dropping qualifiers: "Associate Product Manager" → "Product Manager"

### Required Disclosure

For every title reframing, present to the user:

```
ORIGINAL TITLE: {exact title from their resume}
PROPOSED TITLE: {reframed version}
JUSTIFICATION: {why this is truthful}
LEVEL CHANGE: {Yes/No — if yes, explain why defensible}

Approve this reframing? (Y/N)
```

If the reframed title implies a different job level (e.g., from individual contributor to lead, or from associate to full), flag it explicitly and require stronger justification.

## Content Reframing Rules

Reframing existing bullets is legitimate. The constraints:

### Always Preserve

- The factual core of what happened
- Who was involved (don't inflate team size or stakeholder level)
- The actual outcome (don't upgrade "improved" to "transformed")
- The user's actual role (don't upgrade "supported" to "led")

### Acceptable Changes

- **Keyword alignment:** Change terminology to match JD while preserving meaning
  - "Built automated testing system" → "Implemented CI/CD test automation" (if accurate)
- **Emphasis shift:** Foreground the aspect the JD cares about
  - "Designed experiments and analysed data, saving $2M" → "Prevented $2M in costs through predictive risk detection" (if that's what the analysis did)
- **Abstraction level:** Adjust technical specificity to match the audience
  - "Built MATLAB-based system" → "Developed automated evaluation system" (less specific)
  - OR → "Built automated evaluation system (MATLAB, Python)" (more specific, if both were used)
- **Scale emphasis:** Highlight the relevant dimension of scale
  - "Managed project with 3 stakeholders" → "Led cross-functional initiative across 3 organisational units" (if "units" is accurate)

### Forbidden Changes

- Upgrading verbs beyond what happened (contributed → spearheaded, helped → led, participated → drove)
- Adding outcomes that didn't occur
- Implying broader scope than actual
- Changing "supported" to "owned" or "managed"
- Adding metrics that weren't in the original or provided by the user

### Before/After Transparency

For every reframing, show the user:

```
ORIGINAL: "{original bullet from their resume}"
REFRAMED: "{proposed version}"
CHANGES: {what changed and why}
TRUTHFULNESS CHECK: {why this remains accurate}
```

## Experience Discovery Rules

During the branching interview (Phase 2.5):

### Your Role

- Ask questions to surface what the user actually did
- Help them articulate experiences they struggle to put into words
- Prompt for details: scale, metrics, tools, challenges, outcomes
- Connect dots between different experiences

### Not Your Role

- Fill in blanks they can't answer ("If you don't remember the number, I'll estimate")
- Assume details based on typical roles ("As a PM, you probably did stakeholder management")
- Lead the witness ("That sounds like it was about a 30% improvement, right?")
- Create composite experiences from fragments

### Handling Uncertainty

When the user says something vague:

```
USER: "I think I improved the process somehow"
```

**Wrong:** Write "Streamlined end-to-end process, reducing cycle time by 25%"

**Right:** Ask "What specifically changed? Was it faster, cheaper, more reliable? Do you remember any before/after comparison?"

If they can't be more specific, write something honest: "Improved {process} by {whatever they can confirm}." Leave out what they can't confirm.

### When to Stop

- After 2-3 attempts to surface an experience with nothing substantial: move on
- Some gaps are real — flag them honestly rather than forcing content
- A gap in the resume + a note to address it in the cover letter is better than a fabricated bullet

## The Concreteness Test

Apply this test to every bullet before it goes into the final resume:

1. **Removability test:** Can you remove a phrase and the bullet still means the same thing? Remove it.
2. **Universality test:** Could this phrase appear on anyone's resume? Replace it with something specific to this person.
3. **Defendability test:** Can the user explain this bullet in detail in an interview? If not, simplify.
4. **Metric verification:** Is every number in this bullet sourced from the user's own words or existing resumes? If not, remove it.
5. **Verb accuracy:** Does the action verb match what the user actually did? Not a grander version of it.

## Red Flags to Watch For

During generation, if you find yourself doing any of these, stop and reassess:

- Writing "Spearheaded" or "Championed" — these almost always inflate
- Adding a percentage to a bullet that didn't have one
- Using "transformative", "groundbreaking", "innovative" — promotional language
- Writing a bullet that sounds impressive but you can't explain what specifically happened
- Merging two separate experiences into one bigger-sounding bullet
- Describing what the team did as if the user did it alone
