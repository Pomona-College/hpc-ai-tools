---
title: "Responsible AI Practices"
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do I properly disclose AI tool usage in research?
- How do I verify AI-generated content for accuracy?
- What are hallucinations and how do I detect them?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Properly disclose AI usage in publications and code
- Verify AI-generated claims, citations, and code
- Detect and address AI hallucinations
- Write responsible AI disclosure statements

::::::::::::::::::::::::::::::::::::::::::::::

## Plagiarism and Attribution

Modern AI makes plagiarism easier but also more detectable. Requirements:

- Explicitly disclose AI tool use in your work
- Quote or clearly mark AI-generated text
- Cite the model and version used
- Describe your modifications and verification

**Example disclosure:**
"I used Llama 2 (7B-chat) running on Sagehen HPC to generate initial outlines.
The model produced: [quote]. I then revised and verified all claims with primary
sources."

## Hallucinations and Verification

AI models generate plausible-sounding text that is sometimes false. LLMs
"hallucinate" facts, and confidence is not correlated with accuracy.

**Critical verification steps:**

1. Check factual claims against authoritative sources
2. Test all generated code before using it
3. Verify citations actually exist and match the claims made
4. Treat AI output like a draft that needs fact-checking

::::::::::::::::::::::::::::::::::::: callout

## Hallucinations Affect All Models

Both cloud and local models hallucinate. ChatGPT, Claude, and Llama all
confidently generate false information. Never trust AI output without
verification, especially in research.

::::::::::::::::::::::::::::::::::::::::::::::

## Disclosing AI Use in Publications

**In Methods Section:**

```
Statistical analyses were performed in Python 3.11. For interpretation of
preliminary results, we used Llama-2-7b-chat (Meta) on Pomona's Sagehen HPC
to generate initial hypotheses, which we then tested using formal statistical
tests. All reported results are based on independent analysis of raw data.
```

**In Code Comments:**

```python
# DISCLOSURE: Initial analysis drafted with local Llama model.
# All numerical results independently verified against raw data.
# AI was not used for statistical inference.
```

### Inadequate vs. Adequate Disclosure

- Bad: "We used AI for analysis" (too vague)
- Bad: No mention of AI at all (violates research integrity)
- Good: "ChatGPT was used to draft the introduction, which was then
  extensively revised and cited appropriately."

::::::::::::::::::::::::::::::::::::: callout

## Disclose Even When It Feels Uncomfortable

Transparent disclosure builds trust. If you omit AI use and it is discovered
later, the integrity violation is far more damaging than the admission.

::::::::::::::::::::::::::::::::::::::::::::::

## Environmental Impact

Training and running large models consumes significant energy. Mitigate by:

- Using smaller models when possible (Phi instead of Llama-70B)
- Quantizing models (4-bit instead of 32-bit)
- Batching work for efficiency
- Considering environmental impact when choosing scale

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Audit an AI Output

Get a response from ChatGPT or Claude on a topic in your field. Audit it:

1. Verify each factual claim
2. Look for missing perspectives
3. Identify unstated assumptions
4. Note overconfident claims
5. Write a paragraph describing what you would change before citing it

::::::::::::::::::::::::::::::::::::: solution

## Solution

**Example audit of "What causes ML model failures?"**

ChatGPT said: "Models fail because of insufficient data, poor features, and
overfitting. The solution is more data, better features, and regularization."

**Audit findings:**

- Missing: distribution shift, architecture mismatch, deployment drift
- Assumes "more data" is always possible
- Claims a singular "solution" when multiple exist depending on root cause
- Does not acknowledge that some failures have no simple fix

**Revised:** "ML models fail for multiple reasons including data quality,
feature engineering, overfitting, distribution shift, and organizational
testing failures. Solutions depend on root cause analysis."

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Always disclose AI usage specifically in methods sections and code comments
- Verify all AI-generated claims, citations, and code before use
- AI models hallucinate regardless of whether they are cloud or local
- Transparent disclosure builds credibility; concealment destroys it
- Consider environmental impact when choosing model size and scale

::::::::::::::::::::::::::::::::::::::::::::::
