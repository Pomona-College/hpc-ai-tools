---
title: "Pomona's AI Use Policy"
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- What is Pomona's policy on AI tool usage?
- What are the three data classification tiers?
- What are the consequences of policy violations?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand Pomona's AI tool usage policy and guidelines
- Know the three-tier data classification system
- Recognize consequences of policy violations at each severity level

::::::::::::::::::::::::::::::::::::::::::::::

## Pomona's AI Tool Policy

**Official title:** "Use of AI Tools with Pomona College Data" — guidance on
aligning AI usage with Pomona's data governance structure and policies.

**Where to find it:** listed under Policies and Guidelines on the
[Data Governance page](https://www.pomona.edu/data-governance). The document
itself is in Box and requires campus login.

**Core principle:** data classification determines which tools you can use.

::::::::::::::::::::::::::::::::::::: callout

## This Episode Summarises; the Guideline Governs

What follows is a working summary for teaching. The Data Governance page is
authoritative and is revised as tools and contracts change. Where this lesson
and the current guideline disagree, **the guideline wins** — and please tell
<its-hpc@pomona.edu> so the lesson gets fixed.

The same page also holds the **Data Classifications** document that defines the
tiers used throughout this workshop.

::::::::::::::::::::::::::::::::::::::::::::::

### Key Policy Principles

1. **Data Classification First**: Classify data before using any AI tool
2. **Risk Proportionate**: Higher risk data requires stricter controls
3. **Transparency Required**: Disclose AI usage in research and coursework
4. **Compliance Non-Negotiable**: Federal laws (FERPA, HIPAA) override
   convenience -- no exceptions for "just this once"

### Policy Quick Reference

```
PUBLIC data
  External AI: OK (ChatGPT, Claude, Gemini, etc.)
  Local AI: OK (Sagehen)

PROPRIETARY data
  External AI: NOT OK without written permission
  Local AI: Required

RESTRICTED data (FERPA, medical, export-control, PII)
  External AI: ABSOLUTELY NOT
  Local AI: Required + encryption + audit logs
```

## Consequences of Violations

### Minor (Good Faith Effort)

Scenario: Uploaded proprietary draft to ChatGPT (no FERPA/medical data).
Result: Warning, informal discussion with advisor, required training.

### Moderate (Negligence)

Scenario: Uploaded student work to ChatGPT without verifying anonymization.
Result: Formal warning from Provost, required AI ethics training, possible
publication retraction.

### Severe (Intentional or Reckless)

Scenario: Uploaded student grades/names to ChatGPT knowing it was prohibited.
Result: Disciplinary action up to termination, loss of research funding,
possible legal prosecution, FERPA notification to affected students.

## Who to Ask for Help

| Contact | For |
|---------|-----|
| Your Advisor | Research context, data questions |
| its-hpc@pomona.edu | Sagehen, local AI, data classification |
| OVPR (ovpr@pomona.edu) | Export control, IRB, compliance |
| Library Research Support | Citation, attribution, data management |
| Provost's Office | Academic integrity, policy clarifications |

::::::::::::::::::::::::::::::::::::: callout

## When in Doubt, Ask First

Contact its-hpc@pomona.edu before using AI with any data you are unsure about.
Getting help early prevents problems later. Compliance is a partnership between
you, your advisor, the IRB (if applicable), and IT.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Policy Scenarios

For each scenario, what does Pomona's policy require?

1. You want to use ChatGPT to summarize a published paper
2. You want to analyze unpublished lab results with Claude
3. You want to use AI to detect patterns in student grade data

::::::::::::::::::::::::::::::::::::: solution

## Solution

1. **Published paper (PUBLIC):** External AI is fine. No restrictions.
2. **Unpublished lab results (PROPRIETARY):** Local AI only (Sagehen), unless
   you obtain explicit written permission from all stakeholders.
3. **Student grade data (RESTRICTED/FERPA):** Local AI only, with encryption
   and audit logging. Absolutely no external AI services.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Pomona policy requires data classification before AI tool selection
- PUBLIC data may use external AI; PROPRIETARY requires local AI or written
  permission; RESTRICTED data must never go to external AI
- Violations can result in warnings, retractions, or legal consequences
- When unsure, contact its-hpc@pomona.edu before proceeding

::::::::::::::::::::::::::::::::::::::::::::::
