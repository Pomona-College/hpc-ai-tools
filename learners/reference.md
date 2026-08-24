# Reference: Responsible Use of AI Tools on HPC

## Data Classification Quick Reference

### PUBLIC Data
**Definition**: Information that is already public or could be made public without harm

**Can Use**: ChatGPT, Claude, Bard, or any external AI tool

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
- **Safe Method**: Local Llama on Sagehen, or fully anonymize (no names)

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
   YES → ChatGPT, Claude, Bard, or other external AI is FINE
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

## Cloud AI Tools Comparison

### ChatGPT (OpenAI)
- **Cost**: Free (limited), Paid ($20/month)
- **Quality**: Very good (GPT-4 is top-tier)
- **Speed**: Fast
- **Privacy**: Data sent to OpenAI servers
- **Best for**: General questions, code help, drafting

### Claude (Anthropic)
- **Cost**: Free (limited), Claude Pro ($20/month)
- **Quality**: Excellent (comparable to GPT-4)
- **Speed**: Medium
- **Privacy**: Data sent to Anthropic servers
- **Best for**: Long documents, reasoning, detailed explanations

### Bard (Google)
- **Cost**: Free
- **Quality**: Good (improving)
- **Speed**: Fast
- **Privacy**: Data sent to Google
- **Best for**: Real-time information, Google services integration

### Copilot (Microsoft)
- **Cost**: Free
- **Quality**: Good (uses GPT-4)
- **Speed**: Fast
- **Privacy**: Data sent to Microsoft
- **Best for**: Code generation, Office integration

---

## Local AI Models on Sagehen

### Llama 2 (Meta)
```bash
# Model sizes available
7B:   Fast, okay quality, fits comfortably on an L40S (48GB)
13B:  Better quality, needs 32GB GPU (L40S)
70B:  Best quality, needs 2x A100 (80GB each)
```

### Quick Start
```bash
module load miniconda3   # PyTorch comes from conda: conda activate pytorch_env
conda activate ml_env

python3 << 'EOF'
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM

model_name = "meta-llama/Llama-2-7b-chat-hf"
tokenizer = AutoTokenizer.from_pretrained(model_name, use_auth_token=True)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.float16,
    device_map="auto",
    use_auth_token=True
)

# Use it
inputs = tokenizer("What is machine learning?", return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=100)
print(tokenizer.decode(outputs[0]))
EOF
```

---

## AI Usage Disclosure Template

### For Publications
```
Data Analysis

We used ChatGPT-4 (OpenAI) to generate initial hypotheses for data interpretation,
based on preliminary analysis results. All reported numerical results were
independently verified through formal statistical analysis. ChatGPT was not used
in any statistical inference; its role was limited to hypothesis generation and
preliminary interpretation of already-verified findings.

Model Citation:
OpenAI. (2024). ChatGPT-4 language model. https://openai.com
```

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

**Better Answer**: Use local Llama on Sagehen

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
- Pomona College AI Policy: [institution-specific URL]
- FERPA Summary: https://www2.ed.gov/policy/gen/guid/fpco/ferpa/
- HIPAA Overview: https://www.hhs.gov/hipaa/

### Tool Documentation
- ChatGPT: https://openai.com
- Claude: https://anthropic.com
- Llama: https://ai.meta.com/llama/
- HuggingFace: https://huggingface.co

### Compliance Resources
- Institutional Review Board (IRB) Guide
- Research Integrity Office
- Export Control Regulations (Bureau of Industry and Security)

---

## Useful Commands (Sagehen)

```bash
# SSH into Sagehen
ssh your_netid@sagehen.cs.pomona.edu

# Load PyTorch for local models
module load miniconda3   # PyTorch comes from conda: conda activate pytorch_env

# Create encrypted folder for restricted data
gocryptfs --init /bigdata/lab/<labname>/encrypted
gocryptfs /bigdata/lab/<labname>/encrypted /scratch/$USER/decrypted

# Run local Llama model
python3 local_llama_script.py

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
