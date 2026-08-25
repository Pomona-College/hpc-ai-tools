# Setup: Responsible Use of AI Tools on HPC

## Before the Workshop

### Software and Accounts

- Web browser (Chrome, Firefox, Safari, Edge)
- Optional: Pomona NetID for examples
- Optional: Access to Sagehen HPC (not required, but helpful for local AI demo)

### No Installation Required!
This workshop uses cloud AI assistants and an optional local setup. No special
software needs to be installed in advance.

### Optional: Try the Tools in Advance

If you want to get familiar with external AI tools, the three most commonly used
at Pomona are:

- **ChatGPT** (OpenAI): <https://chatgpt.com>
- **Claude** (Anthropic): <https://claude.ai>
- **Gemini** (Google): <https://gemini.google.com>

Each offers a no-cost tier. Sign in with a personal account for the workshop
exercises — we will only ever use PUBLIC example data in them.

:::::::::::::::::::::::::::::::::::::: callout

## Product Names Change Faster Than This Lesson

Google's assistant was called **Bard** until February 2024, when it was renamed
**Gemini**; `bard.google.com` now redirects to `gemini.google.com`. Similarly,
`chat.openai.com` now redirects to `chatgpt.com`.

Expect names, tiers and model numbers to keep moving. Nothing in this workshop
depends on a particular product — the rules you will learn are about **the data
you are handling**, which is exactly why they outlast the tools.

::::::::::::::::::::::::::::::::::::::::::::::

## During the Workshop

### Tools We'll Use

#### External AI (Cloud-Based)

- ChatGPT, Claude or Gemini for examples
- A personal account is fine; we use PUBLIC data only
- Works from any browser

#### Local AI (Optional, for sensitive data)

- Sagehen HPC access (request from ITS if needed)
- Python 3.10+
- Transformers library (installed during demo)

### Software for Hands-On Activities

#### If You Have Sagehen Access
```bash
# Log in
ssh your_netid@sagehen.hpc.pomona.edu

# Load conda. There is no pytorch/tensorflow module on Sagehen --
# frameworks live inside your conda environment.
module load miniconda3
conda activate ml_env
```

#### If You Don't Have Sagehen Access

No problem! You can:

- Observe demonstrations of local model setup
- Use ChatGPT/Claude for practical examples
- Learn the concepts without hands-on coding

## Workshop Structure

This is primarily a **concepts and policy workshop**, not a coding workshop.

### Part 1: Understanding AI Tools (Classroom)

- Landscape of AI tools available
- How AI systems work (high-level)
- Cloud vs. local AI tradeoffs

### Part 2: Data Governance (Classroom)

- Pomona's data classification system
- Regulatory requirements (FERPA, HIPAA, export control)
- How to classify YOUR data

### Part 3: Responsible Practices (Classroom + Optional Hands-On)

- Bias and fairness in AI
- Documentation and disclosure
- Institutional policies and compliance

### Part 4: Practical Implementation (Optional Hands-On)

- Setting up a local model on Sagehen (for RESTRICTED data)
- Building responsible AI workflows
- Asking for help from ITS

## Key Concepts We'll Cover

### 1. Data Classification (Core Skill)

You'll learn to categorize your research data:

- PUBLIC: Safe with any AI tool
- PROPRIETARY: Local AI only or with written permission
- RESTRICTED: Local AI only + encryption

### 2. Tool Selection (Core Skill)

Given data classification, choose appropriate tool:

- ChatGPT/Claude/Gemini? (Only for PUBLIC)
- A local open-weight model on Sagehen? (For PROPRIETARY and RESTRICTED)

### 3. Compliance (Essential Knowledge)

Understand:

- FERPA (student data) - can't use external AI
- HIPAA (medical data) - can't use external AI
- Export control - research must stay on US servers
- Pomona policy - requires documentation

### 4. Responsible Practices (Essential Skills)

- Verify AI outputs for accuracy
- Disclose AI usage in publications
- Audit for bias
- Document decisions

## Before the Workshop: Self-Assessment

Optional: Rate your familiarity with these concepts:

```
AI Tools:
☐ Never used ChatGPT/Claude
☐ Used once or twice
☐ Regular user
☐ Power user

Data Privacy:
☐ Not familiar with FERPA/HIPAA
☐ Heard of them, unclear on details
☐ Familiar with basics
☐ Work with regulated data regularly

Research Background:
☐ First year graduate student
☐ Mid-career graduate student
☐ Faculty/postdoc
☐ Undergraduate researcher

Your AI Use Case:
☐ Haven't thought about using AI
☐ Considering using AI for research
☐ Already using AI
☐ Not sure if I should be using AI
```

This helps us pitch at the right level!

## Getting Help Before the Workshop

### Questions About:

- **Sagehen access**: its-hpc@pomona.edu
- **Workshop content**: Ask instructor before session
- **Pomona AI policy**: Contact your department chair or OVPR

### Resources to Review (Optional)

- [Use of AI Tools with Pomona College Data](https://www.pomona.edu/data-governance)
  (campus login required; covered in Episode 3)
- FERPA Overview: https://www2.ed.gov/policy/gen/guid/fpco/ferpa/
- HIPAA Basics: https://www.hhs.gov/hipaa/

## Technical Requirements

### Minimum

- Web browser
- Internet connection
- No software installation needed

### Recommended (Optional)

- Notepad or note-taking app (for your own data classification notes)
- Pomona NetID and password (to try local Sagehen examples)

## Accessibility

This workshop is designed to be inclusive:

- Visual aids and text slides
- Spoken descriptions of code examples
- Discussions don't require technical background
- Hands-on activities are optional

Let instructors know if you need accommodations!

## Contact for Setup Issues

Before the workshop, if you encounter any issues:

- **Sagehen access problems**: its-hpc@pomona.edu
- **Browser/tech issues**: Your local IT support
- **Workshop-specific**: Email instructor

## Post-Workshop Resources

After the workshop, continue learning:

- [Use of AI Tools with Pomona College Data](https://www.pomona.edu/data-governance) (full text)
- Local model setup guide
- Compliance checklists
- Case studies and examples

These will be provided at the end of the workshop.