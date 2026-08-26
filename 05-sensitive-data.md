---
title: "Sensitive Data and AI"
teaching: 10
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions

- How do FERPA, HIPAA, and export controls affect AI tool choices?
- How do you protect sensitive data when using AI?
- What compliance frameworks apply to AI workflows?

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Understand FERPA, HIPAA, and export control requirements for AI
- Apply NIST SP 800-171 principles to AI workflows
- Use encryption and local models to protect restricted data
- Create compliance documentation for AI-assisted research

::::::::::::::::::::::::::::::::::::::::::::::

## FERPA (Student Data Protection)

Federal law protecting student privacy. Applies to anyone working with student
educational records.

**Protected information:** Student names linked with IDs, grades, academic
records, course enrollment, disciplinary records, transcripts.

**Penalty:** Institution loses federal funding; personal liability of $1000+
per violation.

**Classroom implications:**

- Do NOT upload student work, grades, or feedback to any external AI service
- Do NOT ask an external AI "What's wrong with this student's paper?"
- DO use a local model on Sagehen HPC, or anonymize completely
- DO ask "What's the grammatical issue in this sentence?" (without student context)

## HIPAA (Healthcare Data)

Applies to researchers working with health/medical data.

**Protected Health Information (PHI):** Patient names, medical record numbers,
health conditions, treatment information, dates of service.

**Penalty:** $100-$50,000 per violation; criminal liability possible.

**Safe approach:** De-identify data (remove all identifiers), use local AI only,
consider HIPAA-compliant platforms, consult the IRB.

## Export Controls (ITAR/EAR)

US government restrictions on sharing certain technology and research.

**Affected areas:** Cryptography, semiconductors, advanced materials, missile
technology, some biotechnology and ML applications.

**Risk:** Uploading export-controlled data to a cloud AI (whose company may
have international employees) constitutes technology export without a license
-- a federal crime.

**Safe approach:** All computation on US infrastructure (Sagehen), no cloud AI,
consult the export control officer.

## Working with Restricted Data on Sagehen HPC

```bash
# Use gocryptfs for encryption at rest
gocryptfs --init /bigdata/lab/<labname>/encrypted_research
gocryptfs /bigdata/lab/<labname>/encrypted_research /mnt/decrypted

# Work with data in /mnt/decrypted
# Data is encrypted at rest in /bigdata

# Use local models for analysis
python3 local_analysis.py --data /mnt/decrypted/data.csv
```

## Compliance Documentation

Before starting any AI-assisted research, complete this checklist:

```
Pre-Research AI Governance Checklist:
[ ] Identified all data types in project
[ ] Classified each type (PUBLIC/PROPRIETARY/RESTRICTED)
[ ] Noted any FERPA, medical, or export-control data
[ ] Confirmed tool selection matches classification
[ ] No RESTRICTED data going to external AI
[ ] Verification plan documented
[ ] Disclosure language prepared
```

::::::::::::::::::::::::::::::::::::: callout

## Compliance Is a Shared Responsibility

Compliance is a partnership: you (understand your data), your advisor (knows
the project context), your IRB (if applicable), and IT (enforces technical
controls). Do not hesitate to ask any of these parties for guidance.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge

## Challenge: Write a Data Handling Policy

For a research project (real or hypothetical), write a brief policy:

1. What data classifications are in your project?
2. Which AI tools will you use and why?
3. How will you document AI usage?
4. How will you secure any restricted data?

::::::::::::::::::::::::::::::::::::: solution

## Solution

**Example -- "Analyzing publication trends in computer science":**

- **Data:** PRIMARY: PUBLIC (published papers from arXiv). METADATA:
  PROPRIETARY (our unpublished analysis).
- **Tools:** ChatGPT for summarizing papers (PUBLIC data). A local model on
  Sagehen for preliminary results (PROPRIETARY data).
- **Documentation:** All interactions logged in experiment log; outputs
  verified before publication.
- **Security:** No restricted data in this project. Preliminary results stored
  on Sagehen until publication.

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- FERPA absolutely prohibits sending student data to external AI
- HIPAA requires de-identification and local AI for medical data
- Export-controlled research must stay on US infrastructure (Sagehen)
- Use gocryptfs for encryption of restricted data at rest
- Document all AI usage with a pre-research governance checklist
- Verify all AI-generated outputs before using in publications

::::::::::::::::::::::::::::::::::::::::::::::

<!-- highlight <labname>/<myusername> placeholders in code blocks; remove if the varnish theme handles this natively -->
<script>(function(){var CSS='.sh-placeholder{color:#c2410c;font-weight:700}[data-bs-theme="dark"] .sh-placeholder,html.dark .sh-placeholder{color:#fdba74}@media (prefers-color-scheme: dark){[data-bs-theme="auto"] .sh-placeholder{color:#fdba74}}';var RX=/<labname>|<myusername>/g;function firstMatch(el){var w=document.createTreeWalker(el,NodeFilter.SHOW_TEXT,null),nodes=[],full='';while(w.nextNode()){nodes.push({n:w.currentNode,s:full.length});full+=w.currentNode.nodeValue;}RX.lastIndex=0;var m;while((m=RX.exec(full))){var s=m.index,e=s+m[0].length,inSpan=false,parts=[];for(var j=0;j<nodes.length;j++){var ns=nodes[j].s,ne=ns+nodes[j].n.nodeValue.length;if(ne<=s||ns>=e)continue;parts.push({node:nodes[j].n,a:Math.max(s-ns,0),b:Math.min(e-ns,nodes[j].n.nodeValue.length)});var p=nodes[j].n.parentNode;while(p&&p!==el){if(p.classList&&p.classList.contains('sh-placeholder')){inSpan=true;break;}p=p.parentNode;}}if(!inSpan&&parts.length)return parts;}return null;}function wrapParts(parts){for(var i=parts.length-1;i>=0;i--){var t=parts[i].node,txt=t.nodeValue,a=parts[i].a,b=parts[i].b;var span=document.createElement('span');span.className='sh-placeholder';span.textContent=txt.slice(a,b);var f=document.createDocumentFragment();if(a>0)f.appendChild(document.createTextNode(txt.slice(0,a)));f.appendChild(span);if(b<txt.length)f.appendChild(document.createTextNode(txt.slice(b)));t.parentNode.replaceChild(f,t);}}function run(){var st=document.createElement('style');st.textContent=CSS;document.head.appendChild(st);document.querySelectorAll('pre,code').forEach(function(el){var guard=0,parts;while((parts=firstMatch(el))&&guard++<500){wrapParts(parts);}});}if(document.readyState==='loading'){document.addEventListener('DOMContentLoaded',run);}else{run();}})();</script>
