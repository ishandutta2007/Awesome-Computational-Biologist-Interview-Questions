# 8. Behavioral & Case Studies

Real-world scenarios and ambiguous problems. Many computational biology interviews include a live problem-solving exercise — practice narrating your reasoning aloud, not just arriving at an answer.

---

### Q: Walk me through how you'd approach a completely new type of biological dataset you've never worked with before, given a specific research question to answer. 🟡

**Case-study structure — a strong candidate would:**
1. **Understand the biology and the data generation process first** — how was this data actually produced, what are its known technical characteristics and limitations, and what has prior literature (if any) established about analyzing this data type?
2. **Look for existing established methods/tools** for this data type before building something from scratch, since re-deriving well-established analysis approaches from first principles is usually a poor use of time compared to adopting and adapting existing, validated methods where they exist.
3. **Perform exploratory analysis and quality control** before committing to a specific analysis approach, to understand the data's actual structure, quality, and any unexpected characteristics.
4. **Identify what could go wrong** specific to this data type (known technical artifacts, common pitfalls documented in the literature) and build appropriate checks into the analysis plan proactively.
5. **Scope an initial, achievable analysis plan** and communicate a realistic timeline, being honest that working with an unfamiliar data type carries more inherent uncertainty than a well-practiced analysis.

**Follow-ups:**
- How would you decide when it's worth the time investment to deeply learn a new data type/method yourself, versus finding a collaborator or existing tool that already has that expertise?

---

### Q: Tell me about a time you had to choose between a fast, simple analysis approach and a more rigorous, time-consuming one under real project time pressure. 🟡

**What a strong answer includes:**
- A specific, honest articulation of the actual tradeoff considered — what rigor was potentially being sacrificed, and how you assessed whether that sacrifice was acceptable given the specific decision the analysis was informing.
- Evidence of communicating this tradeoff transparently to stakeholders/collaborators rather than silently cutting corners without disclosure.
- A concrete example connecting to specific stakes — e.g., "this was an early, exploratory hypothesis-generation analysis where a fast, simpler approach was appropriate, since any promising finding would be validated more rigorously before being trusted" versus a scenario where you insisted on more rigor because the analysis was feeding a higher-stakes decision.
- An honest reflection on whether, in hindsight, the tradeoff was the right call.

**Follow-ups:**
- How do you decide, in general, how much statistical/methodological rigor is "enough" for a given analysis, given that more rigor always costs more time?

---

### Q: Describe a time your initial computational finding didn't hold up under further scrutiny (yours or a collaborator's). What did you learn? 🟡

**What a strong answer includes:**
- Honesty that this is a normal, expected part of rigorous computational biology work — interviewers are wary of candidates who claim every initial finding they've reported held up perfectly.
- A specific, technically grounded account of what the flaw actually was (a confound, a methodological assumption that didn't hold, insufficient statistical power, a batch effect, etc.) — connecting to the concepts discussed throughout this repo.
- Evidence of appropriately updating your conclusion and communicating the correction, rather than being defensive or quietly letting an incorrect finding persist.
- A systemic change made afterward (e.g., a new standard check you now build into similar analyses) to catch a similar issue earlier next time.

**Follow-ups:**
- How did you communicate the retraction or correction of your initial finding to whoever you'd already shared it with?

---

### Q: You're given a differential expression result showing thousands of "significantly" differentially expressed genes between two conditions. How would you approach evaluating whether this result is trustworthy before drawing biological conclusions? 🔴

**Live exercise structure — a strong candidate would:**
1. **Check the experimental design and sample sizes** — is this consistent with a plausible number of true biological differences given the conditions being compared, or does the sheer number of "significant" genes suggest something is off (e.g., an unaccounted-for batch effect or confound dominating the signal, as discussed in section 4/genomics companion repo)?
2. **Examine the statistical methodology used** — was appropriate multiple testing correction applied? Was an appropriate statistical model used for the data type (e.g., a negative-binomial-based method for RNA-seq counts, not a naive t-test)?
3. **Visualize the data directly** (e.g., a PCA or clustering plot) to check whether the two conditions actually separate primarily along the biological axis of interest, or whether some other factor (batch, an unmeasured covariate) appears to be driving the separation instead.
4. **Check effect sizes, not just p-values/significance calls** — are the differentially expressed genes showing biologically meaningful fold changes, or are many of them statistically significant but with very small effect sizes that may not be biologically important?
5. **Cross-check a subset of top hits against prior biological knowledge** — do the top differentially expressed genes make biological sense given what's known about the conditions being studied, or do they look like an unexpected, hard-to-explain pattern that might indicate a technical issue?

**Follow-ups:**
- The PCA plot shows samples separating cleanly by processing batch rather than by the biological condition of interest. What would you do at this point?

---

### Q: Tell me about a time you had to push back on a request (from a PI, manager, or collaborator) for an analysis or interpretation that you believed wasn't statistically or methodologically sound. 🔴

**What a strong answer includes:**
- A specific, concrete example of the disagreement, framed in terms of the actual technical/statistical concern rather than a vague objection.
- Evidence of communicating the concern constructively and with appropriate humility (e.g., proposing an alternative approach or a way to test whether the concern is actually consequential) rather than simply refusing.
- An honest account of how the disagreement was resolved — including situations where you were ultimately overruled, and how you handled that professionally while still standing by your technical judgment.
- Reflection on what this taught you about building credibility so your technical concerns are taken seriously in future disagreements.

**Follow-ups:**
- If you were ultimately overruled and the analysis proceeded as originally requested despite your concerns, how would you handle documenting or communicating your reservations for the record?
