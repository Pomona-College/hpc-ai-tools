# Instructor Notes: Responsible Use of AI Tools on HPC

::::::::::::::::::::::::::::::::::::: callout

## Maintenance: This Lesson Has a Shelf Life

More than any other workshop in the series, this one goes stale. Product names
change (Bard became Gemini in February 2024), models are withdrawn (GPT-5.1 was
removed from ChatGPT in March 2026), and prices move constantly.

The lesson is deliberately built so that **only a small, marked surface needs
maintenance**. Everything else is framed around data classification, which does
not change. Before each delivery, check these and nothing else:

| Location | What to check | Last verified |
|---|---|---|
| `episodes/01-ai-landscape.md` | Model size table, marked "as of August 2026" | 2026-08-25 |
| `episodes/02-types-of-models.md` | Open-weight model list and licences | 2026-08-25 |
| `learners/reference.md` | Cloud tool table; local model table | 2026-08-25 |
| `episodes/03-pomona-ai-policy.md` | Link to Data Governance page still resolves | 2026-08-25 |

Deliberate choices worth preserving if you edit:

- **No quality rankings or leaderboard claims.** They rot within weeks and are
  contested. The lesson ranks nothing.
- **No prices.** Tiers and figures change; the lesson says only that free and
  paid tiers exist.
- **Ungated, permissively licensed models in all examples** (`gpt-oss-20b`), so
  no learner is blocked waiting on a Hugging Face access request mid-workshop.
- **The teaching point is never the model.** If you swap in a newer model, the
  exercises should still work unchanged.

::::::::::::::::::::::::::::::::::::::::::::::

## Workshop Overview

**Duration**: 1 day (3-4 hours contact time)
**Format**: Lecture with discussion and interactive exercises
**Target Audience**: All Pomona researchers (no technical background required)

**Learning Outcomes**: By the end, learners can:
- Classify their research data into Pomona's framework
- Select appropriate AI tools based on data classification
- Understand federal regulations affecting AI use (FERPA, HIPAA, export control)
- Implement responsible AI practices (verification, disclosure, bias auditing)
- Know how to get help from institutional compliance teams

**Key Message**: "Using AI responsibly is everyone's responsibility. When in doubt, ask."

---

## Episode-by-Episode Teaching Notes

### Episode 1: The AI Tools Landscape (15 min teaching + 5 min discussion)
**Goal**: Demystify AI tools and set context for responsibility discussions

**Key Points to Emphasize**:
- AI tools are tools (not magical, not conscious)
- Cloud AI is easy but sends data externally
- Local AI is harder to set up but keeps data private
- Choice depends on your data, not just convenience

**Live Demo**:
- Show ChatGPT/Claude interface (2-3 minute demo)
- Ask it a simple question live
- Show result quality and speed

**Common Misconceptions to Address**:
- "Cloud AI is always better" (False: local AI is safer for sensitive data)
- "AI is perfect" (False: AI makes mistakes, hallucinates, has biases)
- "Nobody can find out if I share data" (False: AI companies log everything)

**Discussion Questions**:
- "Who here has used ChatGPT? What for?"
- "What hesitations do you have about using external AI?"
- "Have you thought about data privacy before?"

**Troubleshooting**:
- If WiFi is slow, use pre-recorded demo
- Have screenshots ready if ChatGPT is down

---

### Episode 2: Pomona College AI Policy (20 min teaching + 10 min exercises)
**Goal**: Help learners understand institutional policy and data classification

**Key Points**:
- Pomona has an official AI policy (not just guidelines)
- Policy is based on data classification
- Different data → different rules
- FERPA is non-negotiable federal law

**Structure of Teaching**:
1. Show the four data classifications (2 min)
2. Explain each with examples (8 min)
3. Show decision tree (3 min)
4. Practice with examples (7 min)

**Critical Concepts to Drive Home**:
- PUBLIC data: "Safe with ChatGPT"
- PROPRIETARY data: "Local AI only, or you break NDAs"
- RESTRICTED data: "NEVER external AI - federal law"

**Use Real Examples**:
- Show Pomona's actual policy document
- Reference current news stories about AI/privacy breaches
- Mention FERPA violations from other institutions

**Interactive Exercise**:
Present 5 scenarios, have learners vote on classification:
1. "Published research paper abstracts" (PUBLIC)
2. "Student course feedback forms with names" (RESTRICTED - FERPA)
3. "Our lab's unpublished methods (no data)" (PROPRIETARY)
4. "Patient survey data from hospital partner" (RESTRICTED - medical)
5. "Anonymized student survey responses" (PROPRIETARY/PUBLIC - explain anonymization nuance)

**Discussion**:
- "What happens if you violate Pomona policy?" (Disciplinary action, retraction, funding loss)
- "Has anyone gotten unclear guidance before?" (Normalize the question!)

**Key Takeaway**:
"If you're unsure about classification, email its-hpc@pomona.edu. That's what they're there for."

---

### Episode 3: Data Privacy and Classification for AI Usage (25 min teaching + 15 min exercises)
**Goal**: Deep dive into data protection; connect policy to actual research decisions

**Teaching Structure**:
1. Decision tree walkthrough (5 min)
2. Real-world classification examples (7 min)
3. Compliance frameworks (NIST, FERPA, HIPAA) (5 min)
4. Documentation templates (3 min)
5. Interactive exercises (15 min)

**Emphasize FERPA**:
- Show what counts as educational record
- Explain consequences (federal law, not just institutional)
- Mention actual FERPA violation cases
- Give clear examples of what to do/not do

**Local AI Setup**:
- Show gocryptfs for encryption (safe storage)
- Explain why encryption matters
- Mention that Sagehen HPC is US infrastructure (export control safe)

**Interactive Activities**:
1. **Scenario Analysis**: Present 3 research projects, classify all data
2. **Policy Application**: Given classification, what AI tools are allowed?
3. **Risk Assessment**: What could go wrong if classification is wrong?

**Discussion**:
- "What data do you work with in YOUR research?"
- "How would you classify it?"
- "Are you confident in that classification?"

**Important Nuance**:
- Anonymization is tricky (difficult to truly anonymize)
- Recommend: "If you have to anonymize, probably shouldn't share"
- Better: Use a local model on Sagehen

**Closing**:
"The principle is: When in doubt, assume stricter interpretation."

---

### Episode 4: Running AI Models Locally on Sagehen HPC (25 min teaching + 20 min optional hands-on)
**Goal**: Show practical path for handling sensitive data safely

**Two Options**:
1. **Hands-On**: Actually run a local model on Sagehen (requires Sagehen access)
2. **Demonstration**: Show pre-recorded demo (safer if no WiFi/access)

**If Hands-On** (assumes SSH access available):
```bash
module load miniconda3   # PyTorch comes from conda: conda activate pytorch_env
# Create environment, load gpt-oss-20b, run query
# Time: 10-15 minutes for first load, 5 seconds for subsequent queries
```

**Key Points**:
- Data stays on Pomona infrastructure
- No external dependency on cloud service
- Trade-off: Takes longer than ChatGPT (acceptable)
- Quantization can speed up (explain 4-bit reduction)

**Demonstrate**:
- Loading model (slow first time, normal)
- Asking a question (show inference speed)
- Comparing to ChatGPT (much slower but private)

**Discussion**:
- "Would you use a local model for sensitive data?" (Yes, despite slowness)
- "What if you need real-time speed?" (Use ChatGPT for PUBLIC data only)
- "Can you combine both?" (Yes: local for sensitive, cloud for public)

**Practical Workflow to Show**:
1. Get research data
2. Classify it
3. If RESTRICTED: Use a local model on Sagehen
4. If PUBLIC: OK to use ChatGPT
5. Always verify outputs

**Troubleshooting**:
- Model download fails? Pre-download and cache
- CUDA not available? Check module loads
- GPU memory error? Use quantized model (4-bit)

---

### Episode 5: Ethical Considerations and Bias in AI Systems (25 min teaching + 15 min exercises)
**Goal**: Critical thinking about AI outputs; not all AI advice is good advice

**Key Concepts**:
1. **AI can encode bias** (from training data)
2. **AI can hallucinate** (make up facts)
3. **AI can be confidently wrong** (sounds authoritative when false)
4. **This matters for research credibility**

**Teaching Structure**:
1. Sources of bias (2 min)
2. Real-world bias examples (5 min)
3. Audit checklist (3 min)
4. Disclosure in publications (5 min)
5. Interactive exercises (15 min)

**Real Examples of AI Bias**:
- Facial recognition worse on darker skin tones
- Hiring algorithms that discriminate based on gender/race
- Medical algorithms that undertreat certain populations
- Language models that reflect stereotypes

**Emphasize**:
- You (the researcher) are responsible for verifying
- If you use AI output, you vouch for accuracy
- Disclosure doesn't excuse poor verification

**Audit Checklist Activity**:
Present AI response, have learners identify problems:

Example 1:
```
AI: "The causes of poverty are primarily individual: laziness and lack
of work ethic account for 80% of poverty."
```
**Problems**:
- Ignores structural factors
- Reflects harmful stereotype
- Contradicts evidence

Example 2:
```
AI: "According to the latest research in 2025, quantum computers have
achieved general artificial intelligence."
(This is false - no such breakthrough exists)
```
**Problems**:
- Confident hallucination
- Cites fake sources
- Spreading misinformation

**Interactive Exercise**:
Ask learners: "If you included this in your paper without verification, what would happen?"
(Answer: Retraction, damage to credibility)

**Disclosure Language**:
Show how to write in methods:
- "ChatGPT was used for hypothesis generation [specific way]"
- "All reported results were independently verified"
- "No AI output was used in statistical inference"

**Discussion**:
- "How do you currently verify sources?" (Apply same rigor to AI)
- "What if AI gets it wrong?" (Your responsibility to check)
- "Should you disclose when you DON'T use AI?" (No, only when you do)

**Key Takeaway**:
"AI is a research tool, like Google Scholar. You wouldn't copy Google Scholar results into your paper without reading them. Same principle for AI."

---

### Episode 6: Institutional Policies, Compliance, and AI Governance (25 min teaching + 15 min exercises)
**Goal**: Connect policy to action; know how to get help

**Teaching Structure**:
1. Regulatory overview (FERPA, HIPAA, export control) (8 min)
2. Compliance workflow (pre-research checklist) (5 min)
3. Consequences of violations (3 min)
4. How to get help (3 min)
5. Case studies (6 min)

**Regulatory Focus**:

**FERPA** (Most common at college):
- What it protects (student records)
- Why it matters (federal law)
- What you can't do (share with external AI)
- What you can do (local model or fully anonymize)
- Real consequences (institution loses federal funding)

**HIPAA** (If medical research):
- What it protects (patient health info)
- Why it matters (privacy + security)
- What you can't do (share with unapproved external AI)
- What you can do (HIPAA-compliant service or local)

**Export Control** (If sensitive research):
- What it protects (US technology)
- Why it matters (national security)
- What you can't do (share with non-US entities)
- What you can do (computation on US infrastructure)

**Pre-Research Checklist**:
Walk through the provided checklist with example project:
```
Project: Analyzing student writing samples

Data: Student essays with names and student IDs
Classification: RESTRICTED (FERPA)

Regulations: FERPA applies
Approved Tools: Local models on Sagehen ONLY
Verification Plan: Manual checking of outputs
Disclosure: Methods section note about AI usage

This checklist prevents problems!
```

**Consequences Matter**:
- Violation = disciplinary action
- Publications can be retracted
- Funding can be lost
- If FERPA: Federal law violated

**Who to Contact**:
Create visual with flowchart:
```
Unsure about classification? → ITS (its-hpc@pomona.edu)
FERPA question? → Registrar's office
HIPAA question? → IRB
Export control? → Provost's office
General policy? → Your advisor
```

**Case Studies**:
Present 3 scenarios, discuss what went right/wrong:

**Case 1: Good Practice**
- Researcher had student data
- Classified as RESTRICTED
- Used a local model on Sagehen
- Documented decision
- → COMPLIANT ✓

**Case 2: Problem but Fixed**
- Researcher started to use ChatGPT with sensitive data
- Realized it was FERPA-protected
- Pivoted to local model
- Still OK → COMPLIANT (after correction)

**Case 3: Violation**
- Researcher sent student data to ChatGPT without checking
- Student found their data online
- Reported to administration
- Paper retracted, disciplinary action
- → VIOLATION ✗

**Discussion**:
- "How would you handle Case 2 if it happened?" (Immediate action to switch to local AI)
- "What early warning signs indicate violation risk?" (Unclear classification, unsure of policy, shortcuts)

**Key Takeaway**:
"Institutional compliance exists to protect you and your research. Ask questions; that's what support staff are for."

---

## Workshop Logistics

### Pre-Workshop Setup
- [ ] Test ChatGPT/Claude access (or have screenshots ready)
- [ ] Ensure Sagehen access available (optional, for demo)
- [ ] Prepare screen recordings as backup (WiFi might fail)
- [ ] Have Pomona's policy document ready to reference
- [ ] Contact ITS to confirm they're available for questions
- [ ] Prepare example scenarios (use real-ish case studies)

### Room Setup
- Projector for slides/demos
- Interactive: Voting/polling tools useful for scenarios
- Optional: Live Sagehen access (requires network)
- Handouts: Copy of decision tree, contact list

### During Workshop
- **Pacing**: Allow time for questions (policy discussions need clarity)
- **Engagement**: Interactive exercises prevent passive listening
- **Realism**: Use real compliance language, not simplified versions
- **Support**: Have ITS rep available for technical questions

---

## Teaching Considerations

### Tone
This workshop covers compliance and responsibility, which can feel heavy. Balance with:
- **Normalize confusion**: "These are legitimate questions"
- **Empower, don't scare**: "You CAN use AI responsibly"
- **Practical focus**: "Here's how to actually do this"

### Audience Diversity
Learners will have very different AI experience:
- Complete beginners: Haven't used ChatGPT
- Regular users: Already using AI in research
- Power users: Asking about advanced use cases

**Teaching Strategy**:
- Beginners: Focus on decision tree and basic concepts
- Experienced: Discuss edge cases, nuances
- Everyone: Understand Pomona's specific policy

### Common Pushback

**"I'm not doing anything sensitive, so it's fine to use ChatGPT"**
- Response: "Great! But how do you know what's sensitive? Let's classify first."

**"Everyone uses external AI with their data"**
- Response: "They might be violating policy. Our institution has specific rules to protect you."

**"Local AI is too slow"**
- Response: "Correct! Trade-off: slower but private. For public data, ChatGPT is fine."

**"This policy limits research"**
- Response: "Actually enables research. Using local AI for sensitive data is liberating, not limiting."

---

## Assessment and Feedback

### Assess Learning Through:
- **Scenario exercises**: Can they classify data correctly?
- **Decision tree**: Can they navigate to right tool?
- **Contact knowledge**: Do they know whom to email?
- **Practical application**: Could they apply this to their own research?

### Feedback Survey Items
```
1. Did you understand Pomona's data classification system?
   (Very unsure / Somewhat unsure / Confident / Very confident)

2. Could you classify your own research data after this workshop?
   (Yes / No / Not applicable)

3. Do you know when to use ChatGPT vs. local AI?
   (Yes / No / Somewhat)

4. What question still remains about AI policy?
   (Open text - helps identify follow-up needs)

5. Would you recommend this workshop?
   (1-5 scale)
```

---

## Common Learner Questions and Answers

**Q: "Can I use ChatGPT to analyze student feedback?"**
A: Only if you remove all identifiers. Better: Use a local model on Sagehen.

**Q: "What counts as identifying?"**
A: Names, student IDs, course numbers, dates, assignment names. If you can trace back to an individual, it's identifiable.

**Q: "Does anonymization make it OK?"**
A: Not really. Safer to just use local AI instead.

**Q: "What if I accidentally send FERPA data to ChatGPT?"**
A: Contact ITS immediately. The faster you act, the better. Don't hide it!

**Q: "Is our lab data proprietary?"**
A: Check with your advisor or department chair. When in doubt, ask ITS.

**Q: "Can I use ChatGPT with data from collaboration?"**
A: Only if partner agrees. Check your NDA!

**Q: "How do I know if research is export-controlled?"**
A: Government websites, funding agency, or ask Provost's office.

**Q: "Is a local model actually secure?"**
A: Yes, it's on US infrastructure and doesn't send data anywhere.

---

## Follow-Up Resources

### For Learners
- Pomona's full AI policy (provide link)
- FERPA for researchers (explain sheet)
- Local model setup guide
- Case studies and examples
- ITS contact information

### For Your Own Development
- Read Pomona's complete policy document
- Contact OVPR for updates on regulations
- Review FERPA/HIPAA resources
- Stay updated on AI tool landscape changes

---

## Post-Workshop Actions

### Immediate
- Distribute slides and reference materials
- Provide ITS contact info
- Share local model setup guide

### Follow-Up (1 week)
- Send summary email with key takeaways
- Provide answers to questions asked during workshop
- Announce any policy updates

### Ongoing
- Collect feedback for improvement
- Update workshop based on emerging AI/policy changes
- Share case studies as they emerge

---

## Notes from Previous Runs

*(Update after each workshop)*

### What Went Well
- Real-world case studies helped people understand consequences
- Interactive scenarios made policy concrete
- Decision tree demystified complex policy
- Showing the local model demo increased confidence

### What to Improve
- FERPA details still unclear for some; consider more examples
- Export control needs more clarity (many researchers don't think it applies)
- Some wanted more technical detail (consider separate advanced session)
- Time: Policy discussion took longer than expected

### Feedback Themes
- "Finally understand what I can and can't do"
- "Glad there's local option for sensitive data"
- "Wish I'd had this before I started my project"
- "Good to know ITS is available for questions"

---

## Critical Reminders for Instructors

1. **You are not a lawyer**: Refer complex legal questions to compliance office
2. **Policy changes**: Confirm Pomona policy is still current before workshop
3. **Emphasize support**: This isn't about punishing violations; it's about enabling research safely
4. **No judgment**: People ask because they care about compliance; that's good
5. **Document everything**: When people ask questions, note them for policy updates