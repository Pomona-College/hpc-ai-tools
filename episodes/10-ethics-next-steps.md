---
title: "Ethical Considerations and Next Steps"
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do AI systems encode bias and what are the consequences?
- How do you audit AI outputs for fairness?
- What are the broader ethical implications of AI in research?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand sources of bias in AI systems
- Implement fairness audits for AI-generated results
- Write broader impact statements for AI-assisted research
- Apply ethical principles to your own research practice

::::::::::::::::::::::::::::::::::::::::::::::

## Sources of Bias in AI

### Training Data Bias

AI trained on historical data inherits historical biases. Example: facial
recognition trained mostly on white faces achieves 95%+ accuracy on light skin
tones but only 20-35% on darker skin tones.

### Label Bias

Human annotators introduce their cultural and linguistic norms into training
labels. Models learn those specific biases and may perform poorly on different
populations.

### Selection Bias

Training data often does not represent the full population. A depression
detection model trained on diagnosed patients misses undiagnosed cases and
atypical presentations.

### Measurement Bias

Proxy metrics (like test scores for "intelligence") encode structural
inequities. Models trained on such metrics replicate and amplify those
inequities.

## Fairness Audits

If using AI to make predictions about people, test accuracy across groups:

```python
import pandas as pd

data = pd.read_csv('test_data.csv')
data['predicted'] = model.predict(data)
data['correct'] = data['actual'] == data['predicted']

for group in data['demographic_group'].unique():
    group_data = data[data['demographic_group'] == group]
    accuracy = group_data['correct'].mean()
    print(f"{group}: {accuracy:.1%}")

# If any group is far below others, there is a fairness problem
```

Document known limitations when you find disparities:

```python
# Known limitation: Group B accuracy is 78% vs 95% for Group A
# Recommendation: Do not use for high-stakes decisions affecting Group B
# until accuracy is improved
```

## Broader Impact Statements

When publishing AI-assisted research, include:

```
BROADER IMPACT:
This research uses [specific AI tool] for [specific purpose].

Potential benefits:
- Faster research iteration
- Wider access to analytical capabilities

Potential risks:
- AI may propagate biases present in [domain]
- Overconfidence in AI hypotheses could mislead research

Mitigation:
- All outputs independently verified
- Tested for bias in [specific ways]
- Should not be deployed in [contexts] without further validation
```

## Principles for Ethical AI Use

::::::::::::::::::::::::::::::::::::: callout

## Seven Principles for AI in Research

1. **Verification First**: Never trust AI output without checking
2. **Transparency**: Disclose usage clearly and specifically
3. **Document Limitations**: Be explicit about what AI cannot do well
4. **Fairness Audits**: Test outputs for bias on protected categories
5. **Responsible Deployment**: Do not use AI where it has not been validated
6. **Attribution**: Credit AI tools as you would human collaborators
7. **Integrity**: AI accelerates research but does not replace verification

::::::::::::::::::::::::::::::::::::::::::::::

## Next Steps

1. **Classify your data** using the decision tree from Episode 4
2. **Set up your environment** on Sagehen HPC following Episode 7
3. **Start small** with Phi-4-mini or an 8B-class model before scaling up
4. **Document everything** -- AI usage logs, verification steps, disclosures
5. **Stay current** with evolving institutional policies and AI capabilities
6. **Ask for help** at its-hpc@pomona.edu whenever uncertain

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Write a Disclosure Statement

For your research project (real or hypothetical):

1. Identify where you might use AI
2. Write a one-paragraph methods disclosure
3. Describe what AI will and will not be used for
4. Explain how you will verify outputs
5. Note any bias concerns

::::::::::::::::::::::::::::::::::::: solution

## Solution

**Example for genomics research:**

"Gene annotation analysis: We used gpt-oss-20b running on Pomona's Sagehen
HPC to generate initial literature review outlines and suggest potential
regulatory elements. These AI-generated suggestions were not used directly;
they served as hypothesis-generation tools. All reported results (promoter
identification, regulatory predictions) were independently identified through
systematic computational screening and validated against published databases.
AI outputs included spurious regulatory elements and outdated citations that
required manual filtering. AI tools were not used in any formal statistical
inference. Bias concern: the model's training data overrepresents well-studied
organisms, so suggestions for understudied species require extra scrutiny."

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- AI systems encode biases from training data, labeling, and measurement choices
- Fairness audits should test accuracy across demographic groups before deployment
- Write broader impact statements acknowledging risks and mitigations
- Maintain research integrity: AI accelerates work but does not replace verification
- Start your AI journey with data classification, small models, and thorough documentation

::::::::::::::::::::::::::::::::::::::::::::::
