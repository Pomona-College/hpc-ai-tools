# Reference: Responsible Use of AI Tools on HPC

## Data Classification Quick Reference

### PUBLIC Data
**Definition**: Information that is already public or could be made public without harm

**Can Use**: ChatGPT, Claude, Gemini, or any external AI tool

**Examples**:
- Published research papers
- Public datasets
- General knowledge questions
- Non-sensitive coursework

**Policy**: No restrictions

---

### PROPRIETARY Data
**Definition**: Information restricted to specific individuals or organizations. Includes unpublished research, draft papers, lab notebooks, internal working documents, and data protected by NDAs or trade secrets.

**Can Use**: Local AI ONLY or external AI with written permission from all stakeholders

**Examples**:
- Draft research (unpublished)
- Internal working documents
- Preliminary results
- Unpublished institutional data
- Patent applications
- NDA-covered research
- Industry partnership data
- Unreleased commercial data

**Policy**: MUST use local model unless explicit written permission obtained; external AI without permission violates agreements

---

### RESTRICTED Data
**Definition**: Highly sensitive information protected by federal law. Requires mandatory AES-256-GCM encryption.

**Can Use**: Local AI ONLY + encryption

**Examples**:
- FERPA: Student names, IDs, grades, educational records
- HIPAA: Patient health information
- PII: Social Security numbers, addresses, phone numbers
- Export-controlled: Research restricted by ITAR/EAR
- Financial: Credit cards, bank accounts

**Policy**: NEVER external AI; use Sagehen + gocryptfs encryption for storage

---

## Federal Regulations Quick Reference

### FERPA (Family Educational Rights and Privacy Act)
- **Protects**: Student educational records
- **Protected Info**: Names, IDs, grades, course enrollment, transcripts
- **Penalty**: Institutional loss of federal funding, personal $1000+ per violation
- **AI Rule**: NEVER use external AI with student data
- **Safe Method**: A local model on Sagehen, or fully anonymize (no names)

### HIPAA (Health Insurance Portability and Accountability Act)
- **Protects**: Patient health information (PHI)
- **Protected Info**: Medical records, health conditions, treatment, insurance
- **Penalty**: $100-$50,000 per violation, criminal liability
- **AI Rule**: NEVER use external AI without HIPAA business associate agreement
- **Safe Method**: Local AI or HIPAA-compliant cloud service

### Export Control (ITAR/EAR)
- **Protects**: US technology and research from unauthorized export
- **Restricted Research**: Cryptography, semiconductors, materials, biotechnology, ML in some cases
- **AI Rule**: NEVER use external AI with export-controlled research
- **Safe Method**: All computation on Sagehen (US infrastructure only)

---

## Decision Tree for AI Tool Selection

```
START: I want to use an AI tool for my research

1. Have I classified my data?
   YES → Continue to 2
   NO → Classify first (see Data Classification section)

2. Is my data PUBLIC?
   YES → ChatGPT, Claude, Gemini, or other external AI is FINE
   NO → Continue to 3

3. Is my data RESTRICTED (FERPA, HIPAA, export-controlled, PII)?
   YES → Use LOCAL AI on Sagehen ONLY (encrypted)
   NO → Continue to 4

4. Is my data PROPRIETARY (patented, NDA, trade secret, unpublished)?
   YES → Use LOCAL AI on Sagehen ONLY (or external with written permission)
   NO → This shouldn't happen if you classified correctly!

RESULT: Chose appropriate AI tool
```

---

## Cloud AI Tools

The table below deliberately does **not** rank these on quality. Rankings change
with every release, and quality is not what decides whether you are allowed to
use a tool. What matters is where the data goes and under what contract.

| Tool | Vendor | Web address | Data goes to |
|------|--------|-------------|--------------|
| ChatGPT | OpenAI | <https://chatgpt.com> | OpenAI |
| Claude | Anthropic | <https://claude.ai> | Anthropic |
| Gemini | Google | <https://gemini.google.com> | Google |
| Copilot | Microsoft | <https://copilot.microsoft.com> | Microsoft |

All four offer a no-cost tier and one or more paid tiers. Prices and tier names
change often enough that this reference does not quote them.

### The distinction that actually matters

**Personal account** — you signed up yourself with an email address. Your input
is governed by consumer terms, which may permit the vendor to retain it and use
it to improve their models. Treat anything you type as though it may be read by
someone else. **PUBLIC data only.**

**Institutionally contracted account** — Pomona has a signed agreement with the
vendor covering data handling, retention and training. The terms, and therefore
what you are permitted to put in, depend entirely on what that specific contract
says.

Do not assume Pomona holds such a contract for a given tool, and do not assume
one contract's terms apply to another tool. Check the current guidance in
[Use of AI Tools with Pomona College Data](https://www.pomona.edu/data-governance)
or ask <its-hpc@pomona.edu> before putting anything that is not PUBLIC into any
cloud AI service.

::::::::::::::::::::::::::::::::::::: callout

## Names Change; the Rules Do Not

Google's assistant was **Bard** until February 2024 and is now **Gemini**.
Model generations turn over every few months. If a tool you are using is not
listed above, that does not make it approved or unapproved — apply the same
question: *where does this data go, and under whose terms?*

::::::::::::::::::::::::::::::::::::::::::::::

---

## Local AI Models on Sagehen

### Sizing rule (this outlasts any model list)

Estimate VRAM from parameter count and precision:

```
VRAM for weights ≈ parameters × bytes-per-parameter

  16-bit (fp16/bf16) → 2 bytes    7B  ≈ 14 GB
   8-bit             → 1 byte     7B  ≈  7 GB
   4-bit             → 0.5 bytes  7B  ≈  3.5 GB

Then add headroom for activations and the KV cache — budget 20-40% on top
for short prompts, and considerably more for long contexts.
```

For a mixture-of-experts (MoE) model, memory is set by **total** parameters, not
the smaller "active" count. A 109B-total model with 17B active still needs the
full 109B in memory.

### Concrete options as of August 2026

Verify sizes and licences on Hugging Face before relying on them.

| Family | Vendor | Licence | Notes |
|--------|--------|---------|-------|
| `gpt-oss-20b` | OpenAI | Apache 2.0 | Runs in ~16 GB; ungated download |
| `gpt-oss-120b` | OpenAI | Apache 2.0 | Fits a single 80 GB A100 |
| Qwen3 | Alibaba | Apache 2.0 | Wide size range, strong multilingual |
| Mistral Small | Mistral AI | Apache 2.0 | Efficient general-purpose |
| Phi-4-mini | Microsoft | MIT | ~3.8B; smallest useful option |
| Gemma 3 | Google | Gemma terms | Multimodal; licence is *not* OSI-approved |
| Llama 4 | Meta | Llama Community | MoE; gated — requires accepting terms |

**Prefer an Apache 2.0 or MIT model** unless you need something specific from a
gated one. They download without an access request, which removes a step that
otherwise blocks workshop exercises, and their terms are far simpler to satisfy
when you publish.

### Quick Start

```bash
module load miniconda3   # frameworks come from conda, not a module
conda activate ml_env

python3 << 'EOF'
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM

# Apache 2.0 and ungated -- no access request needed.
model_name = "openai/gpt-oss-20b"

tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    dtype=torch.bfloat16,
    device_map="auto",
)

inputs = tokenizer("What is machine learning?", return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=100)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
EOF
```

For a **gated** model (Llama, and some others) you must first accept the licence
on its Hugging Face page, create a token, and authenticate:

```bash
huggingface-cli login          # paste your token when prompted
```

Then pass `token=True` to both `from_pretrained` calls. The older
`use_auth_token=` argument still appears in tutorials online but has been
replaced by `token=`.

---

## AI Usage Disclosure Template

### For Publications
```
Data Analysis

We used ChatGPT (OpenAI, GPT-5.4 Thinking, accessed 2026-07-09) to generate
initial hypotheses for data interpretation, based on preliminary analysis
results. All reported numerical results were independently verified through
formal statistical analysis. The model was not used in any statistical
inference; its role was limited to hypothesis generation and preliminary
interpretation of already-verified findings.

Model Citation:
OpenAI. (2026). ChatGPT (GPT-5.4 Thinking) [Large language model].
  https://chatgpt.com
```

::::::::::::::::::::::::::::::::::::: callout

## Name the Model *and* the Date

"ChatGPT" is the product; the model behind it is swapped out regularly and
older versions are withdrawn. GPT-5.1 was removed from ChatGPT in March 2026
and GPT-5.2 in June 2026 — a paper citing only "ChatGPT" gives a reader no way
to know which of those produced the output, and no way to run it again.

Record **product, model version and access date**. That is the most a reader
can use for a hosted model.

A locally run model is much stronger evidence, because you can pin an exact
revision that will still be there later. Cite the repository and commit:

```
Model Citation:
OpenAI. (2025). gpt-oss-20b [Large language model].
  https://huggingface.co/openai/gpt-oss-20b (revision a1b2c3d)
```

This is a real advantage of running models on Sagehen, not merely a compliance
convenience: **your analysis stays reproducible after the vendor moves on.**

::::::::::::::::::::::::::::::::::::::::::::::

### For Code
```python
# AI-assisted interpretation: ChatGPT was used to suggest potential
# optimizations for this sorting algorithm. The original implementation is
# from [source], and ChatGPT suggestions were validated against test suite.
def optimized_sort(data):
    # ... code ...
    pass
```

### For Course Work
```
I used ChatGPT to help debug a syntax error in my Python code. I provided
the error message and received a suggestion to check variable scope, which
resolved the issue. The solution was verified independently by running the code.
```

---

## Pomona Policy Contact Information

### For Questions About:

**Data Classification**:
- Your advisor
- Department chair
- its-hpc@pomona.edu

**FERPA Compliance**:
- Registrar's office
- OVPR (Office of Vice President Research)
- Dean of Students

**HIPAA or Medical Research**:
- Institutional Review Board (IRB)
- OVPR
- Research Compliance office

**Export Control**:
- Provost's office
- Research compliance officer
- its-hpc@pomona.edu

**General AI Policy Questions**:
- Your advisor first
- Then escalate to OVPR if needed
- contact: ovpr@pomona.edu

---

## Common Scenarios and Correct Responses

### Scenario 1: Coursework Help
**Question**: "Can I use ChatGPT to explain a concept in my class?"

**Answer**: YES, if:
- The concept is PUBLIC (published textbook material)
- You disclose AI usage per syllabus
- You don't submit AI output as your own work

---

### Scenario 2: Student Research Data
**Question**: "I have 50 student surveys. Can I use ChatGPT to analyze themes?"

**Answer**: ONLY if:
- You remove all identifying information (names, student IDs)
- Verify anonymization (test removing one line at a time)
- You have participant consent for AI analysis

**Better Answer**: Use a local model on Sagehen

---

### Scenario 3: Draft Writing
**Question**: "Can I use ChatGPT to draft my grant proposal intro?"

**Answer**: YES if:
- You identify it as PUBLIC information (published literature)
- You heavily revise and verify AI output
- You disclose AI usage if required by funding agency

---

### Scenario 4: Commercial Partnership Data
**Question**: "Our industry partner gave us data. Can I analyze it with ChatGPT?"

**Answer**: NO, unless:
- Partnership agreement explicitly allows external AI
- You have written permission from partner
- Data is already public

**Recommended**: Contact OVPR for guidance

---

## Self-Assessment: Are You Following Policy?

Answer these honestly:

- [ ] I've classified all data in my research
- [ ] I know which data classifications allow external AI
- [ ] I'm not sending any RESTRICTED data to external AI
- [ ] I document my AI usage
- [ ] I disclose AI use where appropriate
- [ ] I verify AI output before using it
- [ ] I understand Pomona's policy

**If you checked all boxes**: You're following good practices!

**If you're unsure on any**: Email its-hpc@pomona.edu

---

## External Resources

### Official Policy Documents
- Pomona College AI policy: check current guidance with ITS — <its-hpc@pomona.edu> · <https://www.pomona.edu/its/>
- Data classification rules for Sagehen: [Data Classification and Handling](https://pomona-college.github.io/hpc-data-classification/)
- FERPA Summary: https://www2.ed.gov/policy/gen/guid/fpco/ferpa/
- HIPAA Overview: https://www.hhs.gov/hipaa/

### Tool Documentation
- ChatGPT: <https://openai.com/chatgpt>
- Claude: <https://claude.ai>
- Gemini: <https://gemini.google.com>
- gpt-oss (open weights): <https://openai.com/open-models/>
- Hugging Face model hub: <https://huggingface.co/models>
- Transformers docs: <https://huggingface.co/docs/transformers>

### Pomona Resources
- Data Governance, including **Use of AI Tools with Pomona College Data**
  and **Data Classifications**: <https://www.pomona.edu/data-governance>
- ITS Policies and Guidelines: <https://www.pomona.edu/administration/its/policies>
- Research Computing / Sagehen: <its-hpc@pomona.edu>

### Compliance Resources
- Institutional Review Board (IRB) Guide
- Research Integrity Office
- Export Control Regulations (Bureau of Industry and Security)

---

## Useful Commands (Sagehen)

```bash
# SSH into Sagehen
ssh your_netid@sagehen.hpc.pomona.edu

# Load conda; frameworks live in your conda env, not a module
module load miniconda3
conda activate ml_env

# Create encrypted folder for restricted data
gocryptfs --init /bigdata/lab/<labname>/encrypted
gocryptfs /bigdata/lab/<labname>/encrypted /scratch/$USER/decrypted

# Keep model weights off your /rhome quota
export HF_HOME=/bigdata/lab/<labname>/huggingface_cache

# Run a local model
python3 local_llm_script.py

# Check GPU availability
nvidia-smi
```

---

## Remember

✓ **Classify first** - Know what data you have
✓ **Ask when unsure** - Better safe than sorry
✓ **Document always** - Keep records for audits
✓ **Verify thoroughly** - Don't trust blindly
✓ **Disclose clearly** - Be transparent
✓ **Respect regulations** - FERPA/HIPAA/export-control aren't suggestions

**When in doubt, contact its-hpc@pomona.edu**

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
