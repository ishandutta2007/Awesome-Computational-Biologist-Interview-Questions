# 7. Experimental Design & Wet-Lab Collaboration

Computational biology rarely exists in isolation from experimental biology — this section covers the collaborative, hypothesis-driven skills that connect the two.

---

### Q: How would you get involved early in experimental design (before data is even generated) to make sure the resulting data will actually support the intended computational analysis? 🟡

**Answer:**
- **Clarify the specific question and intended analysis method upfront**, and work backward to determine what experimental design characteristics are needed to support it — e.g., understanding the statistical power requirements (needed sample size, expected effect size) for the intended analysis before the experiment is run, rather than discovering after the fact that the planned sample size is too small to reliably detect the effect of interest.
- **Advocate for proper randomization and blocking of known technical covariates** (e.g., ensuring biological conditions of interest aren't confounded with batch, processing date, or technician — see the batch effect discussion in the genomics-focused companion repo) at the experimental design stage, since this is often impossible to fully correct for computationally after the fact.
- **Discuss what controls are needed** to distinguish the biological signal of interest from technical or confounding variation, and make sure appropriate controls are included in the experimental design rather than assumed to be unnecessary.
- **Communicate in terms the experimental collaborator will find concrete and actionable** — e.g., translating "we need adequate statistical power" into a specific, justified sample size recommendation, rather than a vague statistical caveat that's easy to deprioritize under time/cost pressure.

**Follow-ups:**
- An experimental collaborator tells you the budget only allows for half the sample size you calculated as necessary for adequate statistical power. How would you handle this conversation?

---

### Q: Describe your approach to translating a biological hypothesis from a wet-lab collaborator into a specific, testable computational analysis plan. 🟡

**Answer:**
- **Make sure you genuinely understand the underlying biological question and hypothesis**, not just the surface-level analysis request — collaborators sometimes propose a specific analysis method when what they actually need is a different approach better suited to their real underlying question, and it's part of the computational biologist's job to probe for this rather than executing a request literally without understanding its purpose.
- **Translate the biological hypothesis into an explicit, falsifiable statistical hypothesis** (what specific pattern in the data would support versus refute the biological hypothesis), making underlying assumptions explicit rather than implicit.
- **Identify what data is actually needed/available** to test this hypothesis rigorously, and flag early if available data is insufficient (e.g., inadequate sample size, missing appropriate controls) rather than proceeding with an analysis destined to be inconclusive or misleading.
- **Agree on the analysis plan and success/interpretation criteria before running the analysis**, where feasible, to reduce the risk of post-hoc, results-driven interpretation (e.g., unconsciously adjusting the analysis approach after seeing preliminary results in a way that biases toward a desired conclusion).
- **Plan for how results will be validated or followed up experimentally**, closing the loop back to the wet lab rather than treating the computational analysis as a standalone endpoint.

**Follow-ups:**
- A collaborator asks you to test a hypothesis but you realize their proposed experimental data can't actually rigorously test it as designed. How do you raise this?

---

### Q: How would you communicate a null or negative computational result (e.g., "we didn't find significant evidence for the hypothesized effect") to a collaborator who was hoping for a positive finding? 🟡

**Answer:**
- **Be clear and direct about the actual result** rather than softening it into ambiguity to avoid disappointing the collaborator — a null result is a genuine scientific finding, not a failure, and obscuring it does a disservice to the scientific process.
- **Distinguish clearly between "we found no evidence of an effect" and "we definitively proved there is no effect"** — these are different claims with different statistical justifications (the latter requires demonstrating adequate statistical power to detect an effect of a biologically meaningful size, not just failing to reach significance), and conflating them is a common and important error to avoid.
- **Discuss possible explanations together**, including genuine absence of the biological effect, inadequate statistical power/sample size, an analysis approach that wasn't well-suited to detecting the effect, or data quality issues — helping the collaborator understand which of these is most likely given the specifics of the analysis, rather than leaving them to draw their own possibly incorrect conclusions.
- **Discuss constructive next steps** (a follow-up experiment with more power, an alternative analysis approach, or accepting the null result as a genuine finding) collaboratively, rather than simply delivering the negative news without a path forward.

**Follow-ups:**
- How would you help a collaborator distinguish between "our analysis found no significant effect" and "we've conclusively shown there's no effect," and why does this distinction matter for how they present the finding?

---

### Q: Tell me about how you'd prioritize which of several proposed computational analyses to pursue first, given limited time and a collaborator eager to see results on all of them. 🟡

**Answer:**
- **Assess which analyses are most likely to yield a clear, interpretable, and actionable result** given the available data quality and sample size, rather than defaulting to whichever the collaborator seems most enthusiastic about, since enthusiasm doesn't correlate with feasibility.
- **Consider dependencies between analyses** — some analyses may need to happen first because their results (or necessary QC steps) inform how a later analysis should be approached.
- **Weigh scientific/business impact and urgency** — is one analysis tied to an upcoming deadline, publication, or decision point that makes it more time-sensitive than the others?
- **Communicate the prioritization rationale transparently to the collaborator**, framing it as maximizing the chance of a genuinely useful result within the available time, rather than simply announcing a priority order without explanation (which can understandably feel arbitrary or dismissive of their other interests).
- **Look for efficient sequencing** — e.g., a shared QC or preprocessing step that multiple proposed analyses depend on might be worth doing first regardless of which specific downstream analysis is prioritized, since it unblocks progress on several fronts at once.

**Follow-ups:**
- The collaborator pushes back and insists all the analyses are equally urgent. How would you navigate this while still making progress?

---

### Q: Describe a scenario where computational and experimental evidence for a hypothesis conflicted with each other. How would you approach reconciling them? 🔴

**Answer:**
- **Don't automatically assume either the computational or the experimental result is "more correct"** — both are subject to their own distinct sources of error (computational: methodological assumptions, data quality issues, statistical artifacts; experimental: assay-specific artifacts, biological variability, technical execution issues) and a mature approach investigates both rather than reflexively trusting one over the other.
- **Look for a plausible source of discrepancy specific to each type of evidence** — e.g., could the computational prediction be based on an assumption that doesn't hold for this specific biological context, or could the experimental result be affected by an assay-specific artifact or an underpowered/small sample size?
- **Design a follow-up experiment or analysis specifically aimed at discriminating between the competing explanations**, rather than simply reporting the discrepancy as unresolved or picking whichever result is more convenient for the narrative.
- **Maintain scientific humility and transparency about the unresolved conflict** if it can't be definitively reconciled with available resources/time, rather than quietly favoring one result without clear justification — an honestly reported discrepancy is more scientifically valuable than a falsely resolved one.

**Follow-ups:**
- How would you decide whether a discrepancy between computational prediction and experimental result is scientifically important enough to warrant further investigation, versus an acceptable level of noise/uncertainty given the methods involved?
