---
title: "Local vs Cloud AI"
teaching: 10
exercises: 5
---

:::::::::::::::::::::::::::::::::::::: questions

- When should I use cloud AI vs. local AI on Sagehen HPC?
- What are the tradeoffs in quality, speed, privacy, and cost?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Compare cloud and local AI on privacy, quality, speed, and cost
- Apply the decision framework to real research scenarios
- Understand when each approach is most appropriate

::::::::::::::::::::::::::::::::::::::::::::::

## Cloud AI (ChatGPT, Claude, Gemini)

**Pros:** the strongest available models, no setup required, instant responses,
continuous improvement, works from anywhere.

**Cons:** data sent to external servers, no control over data retention,
cannot use RESTRICTED data, subscription costs, API rate limits, and **the
model can change or be withdrawn underneath you** — which breaks
reproducibility.

**Best for:** PUBLIC data, quick answers, rapid prototyping.

## Local AI (Run on Sagehen HPC)

**Pros:** Data stays on Pomona infrastructure, can use RESTRICTED and
PROPRIETARY data, no subscription cost, full control, reproducible results.

**Cons:** generally smaller models than the leading hosted ones, setup
required, slower responses (GPU-dependent), GPU queue wait times.

**Best for:** RESTRICTED data, sensitive research, reproducible pipelines,
batch processing.

## Decision Framework

```
Use Cloud AI (ChatGPT/Claude) when:
  Data is PUBLIC
  Speed is critical
  You need the strongest available model
  One-off questions

Use Local AI (Sagehen) when:
  Data is RESTRICTED or PROPRIETARY
  Reproducibility matters
  Privacy is critical
  Batch processing many documents
  Cost matters at scale
```

## When to Use Each: Examples

**Cloud AI appropriate:**
- Drafting and editing text with ChatGPT (non-sensitive)
- Exploring general research questions
- Code generation and debugging
- Summarizing published articles

**Local AI required:**
- Analyzing student survey data (FERPA)
- Processing lab proprietary datasets
- Fine-tuning on domain-specific text
- Running vision models on medical images
- Batch processing hundreds of files

::::::::::::::::::::::::::::::::::::: callout

## Cloud AI Concerns to Remember

- **Data privacy:** All input is sent to external servers
- **Data retention:** Unclear long-term data handling by providers
- **Cost at scale:** Expensive for thousands of queries
- **Dependency:** Service changes or shutdowns affect your work
- **Cannot use:** FERPA, medical data, export-controlled information,
  unpublished proprietary data

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: AI Tool Selection Matrix

For each scenario, decide: cloud AI, local AI on Sagehen, or "don't use AI":

1. Analyzing published climate data for weather predictions
2. Writing code to analyze student mental health surveys (with student IDs)
3. Generating creative text for a novel
4. Summarizing your lab's unpublished methods
5. Creating visualizations of a proprietary compound database
6. Translating published papers from Spanish to English

::::::::::::::::::::::::::::::::::::: solution

## Solution

1. **Climate data (PUBLIC):** Cloud AI acceptable.
2. **Student surveys with IDs (RESTRICTED/FERPA):** Local AI ONLY on Sagehen.
3. **Creative text (no research data):** Cloud AI fine.
4. **Unpublished methods (PROPRIETARY):** Local AI preferred; external only
   with PI written permission.
5. **Proprietary database (PROPRIETARY):** Local AI only or request approval.
6. **Published papers (PUBLIC):** Cloud AI acceptable.

Data classification -- not AI capability -- determines the choice.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Cloud AI is faster and higher quality but sends data externally
- Local AI on Sagehen keeps data private and is cost-effective at scale
- Data classification determines which approach is allowed
- Cloud AI is never acceptable for RESTRICTED or uncleared PROPRIETARY data
- Build workflows that use cloud AI for public data and local AI for sensitive data

::::::::::::::::::::::::::::::::::::::::::::::
