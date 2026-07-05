# 6. Scientific Computing & Reproducibility

The software engineering and computational infrastructure skills that separate a research script from a durable, trustworthy scientific tool.

---

### Q: What software engineering practices do you think are most important for computational biology code, given that it's often written by scientists without formal software engineering training? 🟡

**Answer:**
Practices I'd prioritize, roughly in order of impact relative to effort:
- **Version control (e.g., git) for all analysis code**, even for "just a quick analysis script" — the number of times an earlier version of an analysis needs to be revisited or reproduced is consistently underestimated at the time the code is first written.
- **Clear documentation of what a script/pipeline does, its inputs/outputs, and any key assumptions**, since scientific code is frequently revisited or handed off much later than typical software, often after the original author has forgotten the details or moved on.
- **Modular, reusable functions rather than long, monolithic scripts** — makes code easier to test, debug, and reuse across related analyses, which is common in research settings where similar analyses get repeated with variations.
- **At least basic automated testing for core, reused functions** (even if full test coverage of every one-off analysis script isn't realistic) — particularly important for any function/module that will be reused across many analyses or that implements a non-trivial algorithm, where a subtle bug could silently propagate incorrect results across many downstream analyses.
- **Pinning software/package versions and environment configuration** (see also section on reproducibility below) rather than relying on "whatever happens to be installed."

I'd calibrate the rigor applied to the stakes and reuse likelihood of the code — a one-off exploratory script warrants less investment than a pipeline that will be run repeatedly or that other people will depend on.

**Follow-ups:**
- How would you introduce better software engineering practices to a research group that's used to writing one-off, undocumented analysis scripts, without it feeling like unnecessary overhead to them?

---

### Q: What does it mean for a computational biology analysis to be reproducible, and what specific technical practices support this? 🟡

**Answer:**
Reproducibility means that someone else (or you, much later) can rerun an analysis and obtain the same results, given the same input data — this requires more than just having the analysis code available; it requires the full computational environment and exact parameters to be adequately specified.

Supporting practices: **containerization or environment management** (e.g., Docker/Singularity containers, conda/virtual environments) to pin exact software package versions, since even minor version differences in statistical or bioinformatics tools can occasionally produce different results; **workflow management systems** (e.g., Snakemake, Nextflow) that explicitly define and track the full sequence of analysis steps, their parameters, and their dependencies, rather than relying on manually run, loosely documented scripts; **fixing random seeds** for any stochastic algorithms where exact numerical reproducibility matters; **version-controlling and clearly documenting reference data versions** (reference genomes, annotation files, external database versions) used in the analysis, since these change over time and can meaningfully affect results; and **archiving the exact code, environment specification, and (where feasible) input data** used to generate results reported in a publication or key internal report, rather than only archiving the final results themselves.

**Follow-ups:**
- A collaborator says they can't reproduce a result from your analysis six months later. What's your systematic process for diagnosing why?

---

### Q: What is high-performance computing (HPC), and what practical skills does a computational biologist need to effectively use shared HPC/cluster resources? 🟢

**Answer:**
HPC refers to computing infrastructure (typically a cluster of many interconnected machines/nodes, or cloud-based equivalents) that allows large computational jobs to be distributed and run in parallel, or run with substantially more compute/memory resources than a single desktop/laptop can provide — commonly necessary for many computational biology tasks (large-scale sequence alignment, genome assembly, large simulation studies) that would be impractically slow or memory-infeasible on a single machine.

Practical skills needed: understanding **job scheduling systems** (e.g., Slurm or similar cluster schedulers) to submit, monitor, and manage compute jobs on a shared, multi-user cluster, including specifying appropriate resource requests (CPU cores, memory, time limits) — under-requesting resources can cause jobs to fail, while over-requesting wastes shared resources and can be actively discouraged/penalized on shared academic or institutional clusters; basic understanding of **parallelization strategies** (e.g., splitting a large analysis into independent parallel sub-jobs, such as processing each chromosome or each sample separately) to make effective use of available parallel compute; and awareness of **storage and I/O considerations** (managing large genomic datasets efficiently, understanding the difference between fast scratch storage and slower long-term storage on typical HPC systems) since I/O bottlenecks are a common, underappreciated source of pipeline slowness in genome-scale analyses.

**Follow-ups:**
- How would you decide how much memory and compute time to request for a new type of analysis job you haven't run before on a shared cluster?

---

### Q: How would you approach debugging a bioinformatics pipeline that produces an unexpected or clearly incorrect result somewhere in a long chain of processing steps? 🟡

**Answer:**
- **Isolate the failure to a specific step** by inspecting intermediate outputs at each stage of the pipeline, rather than only looking at the final output — a systematic step-by-step check (does this intermediate file look reasonable given what I'd expect at this stage?) is far more efficient than guessing which step is responsible.
- **Test on a small, well-understood input** where you can manually verify the expected correct output, rather than only debugging against the full, complex real dataset where the correct answer isn't independently known.
- **Check for common, "boring" causes first** before assuming a subtle algorithmic bug — mismatched reference genome/annotation versions, incorrect file paths, silently swapped sample labels, or an unexpected default parameter value are all far more common real-world causes of incorrect results than a genuine bug in well-established, widely-used bioinformatics tools.
- **Check tool documentation and version-specific release notes** for the specific tools involved, since unexpected behavior sometimes reflects a documented but easily-missed default parameter or a version-specific behavior change rather than a true bug.
- **Add appropriate logging/assertions at key pipeline steps** going forward once the issue is found and fixed, so similar issues are caught automatically and immediately in the future rather than requiring the same manual debugging process to be repeated.

**Follow-ups:**
- How would you build systematic sanity checks into a pipeline so that an obviously wrong intermediate result (e.g., zero variants called for an entire chromosome) is flagged automatically rather than silently propagating to the final output?

---

### Q: What's your approach to writing code that needs to scale from a small test dataset during development to a much larger production dataset (e.g., whole-genome or whole-cohort scale)? 🟡

**Answer:**
- **Design and test with realistic scaling considerations in mind from the start**, rather than writing code that only works efficiently at small test scale and discovering scaling problems only once deployed against the full dataset — e.g., being deliberate about avoiding algorithms/data structures with poor scaling behavior (see section 1 on complexity) for any step that will run against genome- or cohort-scale data.
- **Profile actual bottlenecks empirically** rather than guessing where the performance problems will be — it's common for the actual bottleneck (e.g., I/O, a specific unexpectedly slow library call) to differ from initial assumptions.
- **Test on a range of intermediate-sized datasets**, not just the smallest test case and the full production dataset, to catch scaling issues progressively rather than being surprised by a sudden failure only at full scale.
- **Consider memory footprint explicitly**, not just runtime — a script that runs correctly but exhausts available memory at production scale is a common and sometimes harder-to-diagnose failure mode than simple slowness.
- **Build in appropriate checkpointing/restart capability** for long-running production-scale jobs, so a failure partway through a multi-hour or multi-day job doesn't require restarting the entire computation from scratch.

**Follow-ups:**
- You've written a script that works well on a test dataset with 10 samples, but it needs to scale to a cohort of 10,000 samples. What would you specifically check and change before running it at that scale?
