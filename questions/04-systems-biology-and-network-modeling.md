# 4. Systems Biology & Network Modeling

Modeling biology at the level of interacting components — gene regulatory networks, pathways, and dynamical systems.

---

### Q: What is a gene regulatory network (GRN), and what are the main computational approaches to inferring one from expression data? 🟡

**Answer:**
A gene regulatory network represents the regulatory relationships between genes (and their protein products) — typically depicted as a graph where nodes are genes/regulators and edges represent regulatory influence (e.g., a transcription factor activating or repressing a target gene) — capturing how gene expression is coordinately controlled within a cell.

Common computational inference approaches: **correlation/co-expression-based methods** identify genes with correlated expression patterns across conditions/samples as candidate regulatory relationships — simple and scalable, but correlation alone can't distinguish direct regulatory relationships from indirect ones (e.g., two genes both regulated by a common third factor, producing a spurious apparent correlation between them) or determine causal direction; **information-theoretic methods** (e.g., approaches based on mutual information) aim to capture more general (including non-linear) dependencies than simple linear correlation, sometimes combined with methods to prune indirect relationships; **methods leveraging perturbation data** (e.g., gene knockout/knockdown experiments) can provide much stronger causal evidence than purely observational expression data, since directly perturbing a candidate regulator and observing downstream effects is a much more direct test of a causal regulatory relationship than correlation-based inference alone.

**Follow-ups:**
- Why is inferring a truly causal (not just correlational) gene regulatory network from purely observational expression data fundamentally difficult, and how does incorporating perturbation data help address this?

---

### Q: What is the difference between a Boolean network model and an ODE (ordinary differential equation)-based model of a biological system, and when would you choose each? 🔴

**Answer:**
**Boolean network models** represent each component (e.g., a gene) as having a simplified discrete state (typically "on"/"off"), with update rules (logical functions of other components' states) determining how each component's state changes over discrete time steps — a coarse but computationally tractable abstraction of a regulatory system's qualitative dynamics.

**ODE-based models** represent component levels (e.g., concentrations of a protein or mRNA) as continuous variables, with differential equations describing their rates of change as a function of other variables' current levels (e.g., following mass-action or Michaelis-Menten-style kinetics) — capable of capturing more quantitatively realistic, continuous dynamics (including phenomena like oscillations, graded responses, and precise timing), but requiring many more parameters (rate constants, etc.) that are often difficult to measure or estimate reliably, especially for larger networks.

Choice depends on: **data availability** (ODE models need quantitative kinetic parameters that are often unavailable or hard to estimate confidently, especially at larger network scale, while Boolean models can often be constructed from more qualitative/logical knowledge of regulatory relationships); **the biological question** (Boolean models are often sufficient and more tractable for studying qualitative network behavior like attractor states or general logical consistency of a proposed regulatory circuit, while ODE models are necessary when precise quantitative dynamics, timing, or continuous dose-response behavior are the actual question of interest); and **network size** (Boolean and other simplified formalisms scale more easily to larger networks than fully parameterized ODE systems, which become increasingly difficult to parameterize and interpret as the number of interacting components grows).

**Follow-ups:**
- If you had a large regulatory network with many components but very limited quantitative kinetic data, which modeling approach would you lean toward, and why?

---

### Q: What is pathway enrichment analysis, and what statistical pitfalls should you be careful of when interpreting its results? 🟡

**Answer:**
Pathway enrichment analysis tests whether a given gene set of interest (e.g., genes found to be differentially expressed in an experiment) shows statistically significant overlap with predefined biological pathways/gene sets (e.g., from curated pathway databases), aiming to identify which biological processes or pathways are most likely relevant to the observed experimental signal.

Pitfalls to watch for:
- **Multiple testing:** many pathways are typically tested simultaneously, so appropriate multiple-testing correction (see the genomics-focused discussion of FDR control) is essential, not optional.
- **Gene set overlap and redundancy:** many curated pathway definitions overlap substantially with each other (shared genes across related pathways), so multiple "significant" pathways in a results list may actually be capturing largely the same underlying signal rather than independent biological insights — interpreting a long list of "significant" pathways naively as independent findings can be misleading.
- **Background gene set choice matters a lot:** the statistical significance of enrichment depends on what background/reference gene set is used for comparison (e.g., all genes in the genome versus only genes that were actually measurable/tested in the specific experimental platform) — using an inappropriate background is a common and serious methodological error that can produce misleading significance results.
- **Pathway database quality and currency varies:** pathway annotations are curated resources that can be incomplete, somewhat inconsistent between databases, or not fully updated to reflect the latest biological knowledge, so results should be interpreted with appropriate awareness of the underlying annotation resource's limitations, not treated as ground truth.

**Follow-ups:**
- Why is choosing the correct background gene set (rather than defaulting to "all genes in the genome") so important for a valid pathway enrichment analysis?

---

### Q: What is the concept of network motifs in biological networks, and why might their overrepresentation be biologically meaningful? 🔴

**Answer:**
Network motifs are small, recurring subgraph patterns (specific small configurations of nodes and connecting edges) that appear significantly more frequently in a real biological network than would be expected in an appropriately randomized network with the same basic structural properties (e.g., same overall number of nodes and edges, or same degree distribution) — the overrepresentation itself (relative to a proper random null model) is what defines something as a "motif" of interest, not just any recurring pattern.

Their overrepresentation is considered potentially biologically meaningful because certain small circuit patterns are believed to perform specific, useful dynamical/regulatory functions — for example, certain feedback or feedforward loop configurations are associated with functions like generating robust switch-like responses, filtering out transient noise, or enabling specific temporal response dynamics — and their recurring, statistically significant overrepresentation across evolutionarily distant biological networks has been interpreted by some researchers as suggestive of convergent evolutionary selection for these specific useful dynamical functions, though this interpretation and its generality remain areas of ongoing scientific discussion rather than fully settled consensus.

**Follow-ups:**
- What would an appropriate randomized null model need to preserve (e.g., about node degree) to make a network motif overrepresentation claim statistically meaningful, rather than just an artifact of basic network structure?

---

### Q: How would you approach integrating multiple types of omics data (e.g., transcriptomics, proteomics, metabolomics) to build a more complete systems-level model of a biological process? 🔴

**Answer:**
- **Be explicit about what each data type actually measures and its specific limitations** — e.g., transcript levels don't always correlate strongly with protein levels (due to post-transcriptional and translational regulation), so naively assuming one omics layer is a reliable proxy for another can introduce systematic errors into an integrated model.
- **Account for the different timescales and dynamics** each omics layer reflects — transcriptomic changes can occur relatively quickly, while some proteomic or metabolomic changes may lag or have different characteristic response times, which matters for how you'd temporally align or interpret multi-omics measurements taken at the same nominal time point.
- **Use appropriate statistical/computational integration methods** designed for multi-omics data specifically (rather than naively concatenating differently-scaled, differently-distributed data types), being mindful of each layer's distinct noise characteristics and dynamic range (similar considerations to the multi-omics integration challenges discussed for single-cell data in the genomics-focused companion repo).
- **Ground the integration in a specific, testable biological hypothesis or question** where possible, rather than a purely exploratory "let's combine everything and see what emerges" approach, since undirected multi-omics integration without a clear hypothesis is prone to producing statistically fragile, hard-to-validate patterns.
- **Plan for experimental validation of key integrated findings**, since a systems-level model built from correlative multi-omics integration alone provides hypotheses about the underlying biology, not proof, and should ideally be checked against targeted follow-up experiments.

**Follow-ups:**
- Why might transcript-level and protein-level measurements for the same gene sometimes show weak or even contradictory correlation, and what does that imply for how confidently you can integrate these two data types?
