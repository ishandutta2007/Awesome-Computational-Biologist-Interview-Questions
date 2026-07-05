# 2. Sequence Analysis & Phylogenetics

Comparing sequences across genes, species, and evolutionary time — one of the founding problem areas of computational biology.

---

### Q: What is multiple sequence alignment (MSA), and why is it a much harder computational problem than pairwise alignment? 🟡

**Answer:**
Multiple sequence alignment aligns three or more sequences simultaneously, arranging them so that homologous (evolutionarily related) positions across all sequences line up in the same columns, typically with gaps inserted as needed to accommodate insertions/deletions across the sequence set.

It's dramatically harder than pairwise alignment because the dynamic programming approach used for pairwise alignment generalizes to a multi-dimensional scoring matrix whose size grows exponentially with the number of sequences being aligned — an exact, optimal MSA via full dynamic programming is computationally infeasible for more than a handful of sequences. In practice, MSA tools rely on heuristic approaches (most commonly **progressive alignment**, which builds the final alignment incrementally by first aligning the most similar sequence pairs and then progressively adding more distantly related sequences, guided by an initial rough phylogenetic guide tree) rather than an exact, exhaustive algorithm — meaning MSA results are inherently heuristic approximations, not guaranteed-optimal solutions, and different tools/parameter choices can produce meaningfully different alignments for the same input sequences.

**Follow-ups:**
- What's a risk of using a poorly estimated initial guide tree in a progressive alignment approach, and how do more advanced MSA methods try to mitigate this?

---

### Q: Explain the difference between orthologs, paralogs, and homologs, and why this distinction matters for interpreting comparative genomics results. 🟢

**Answer:**
- **Homologs:** genes/sequences that share a common evolutionary ancestor (the broadest category).
- **Orthologs:** homologous genes in different species that arose from a speciation event — typically (though not always) retain the same or similar biological function across species, making them the natural genes to compare when studying, for example, how a gene's function is conserved or has diverged across species.
- **Paralogs:** homologous genes within the same species (or that arose from a gene duplication event) — often diverge in function over evolutionary time, sometimes substantially, even though they share sequence similarity.

This distinction matters because conflating orthologs and paralogs is a common and serious source of error in comparative genomics — e.g., assuming a paralog in another species performs the same function as a gene of interest (rather than correctly identifying the true ortholog) can lead to incorrect functional inference or mistaken evolutionary conclusions. Accurately distinguishing them generally requires proper phylogenetic analysis (reconstructing the gene tree and reconciling it with the species tree), not just sequence similarity alone, since paralogs can sometimes be more sequence-similar to each other than a true, more functionally relevant ortholog.

**Follow-ups:**
- Why can relying purely on sequence similarity (e.g., "the top BLAST hit") to identify orthologs sometimes lead you to incorrectly pick a paralog instead?

---

### Q: How do phylogenetic tree-building methods like maximum likelihood and Bayesian inference differ from simpler distance-based methods (e.g., neighbor-joining)? 🟡

**Answer:**
**Distance-based methods** (e.g., neighbor-joining) first reduce sequence data to a matrix of pairwise evolutionary distances (estimated from sequence differences, often using an explicit model of nucleotide/amino acid substitution to correct for multiple substitutions at the same site) and then build a tree that best fits these pairwise distances — computationally fast, but this compression into pairwise distances discards some information present in the full multiple sequence alignment.

**Maximum likelihood** methods instead directly evaluate, for a given tree topology and set of branch lengths, the likelihood of observing the actual sequence data under an explicit statistical model of sequence evolution, and search across possible tree topologies/parameters to find the one maximizing this likelihood — generally more statistically rigorous and accurate than distance-based methods, at significantly higher computational cost, especially as the number of sequences/taxa grows.

**Bayesian inference** methods similarly use an explicit evolutionary model but sample from a full posterior probability distribution over trees (typically via Markov chain Monte Carlo methods) rather than finding a single best-fit tree — this naturally provides a principled measure of confidence/support for different parts of the tree (posterior probabilities for specific clades) as a direct byproduct of the method, though at substantial computational cost and with its own considerations around prior choice and MCMC convergence diagnostics.

**Follow-ups:**
- Why might you still prefer a fast distance-based method like neighbor-joining for certain use cases despite its lower accuracy compared to maximum likelihood or Bayesian methods?

---

### Q: What is bootstrap support in the context of a phylogenetic tree, and how should it be interpreted (and not over-interpreted)? 🟡

**Answer:**
Bootstrap support is a common way to assess confidence in specific branches/clades of an inferred phylogenetic tree: the original multiple sequence alignment is resampled with replacement (creating many pseudo-replicate datasets), a tree is inferred from each resampled dataset, and the bootstrap support value for a given clade is the percentage of these resampled trees that recover that same clade — higher values generally indicate stronger, more robust support for that particular grouping in the data.

Interpretation caveats: bootstrap support reflects **consistency of the signal within the specific dataset used**, not necessarily biological "truth" — a clade could have high bootstrap support due to a strong but ultimately misleading (non-phylogenetic) signal in the data (e.g., convergent evolution, or a systematic model misspecification affecting all resampled datasets similarly), so bootstrap support shouldn't be treated as an absolute guarantee of correctness. There's also ongoing methodological debate and evolving conventions about what threshold should be considered "well-supported" (commonly, though not universally, values around 70-95% or higher are treated as reasonably strong support), and this should be treated as a rough heuristic rather than a rigid, universally agreed-upon rule.

**Follow-ups:**
- What is "long-branch attraction," and why is it a specific concern for interpreting phylogenetic trees even when bootstrap support looks strong?

---

### Q: What is a sequence motif, and how would you approach discovering statistically significant, previously unknown motifs in a set of biological sequences (e.g., transcription factor binding sites)? 🟡

**Answer:**
A sequence motif is a short, recurring sequence pattern with some biological significance — e.g., a transcription factor binding site, which is typically represented not as a single fixed sequence but as a position weight matrix (or similar probabilistic representation) capturing the pattern of preferred (and tolerated variant) nucleotides at each position.

For de novo motif discovery (finding previously unknown motifs), a general approach: gather a set of sequences believed to share a common regulatory element (e.g., promoter regions of co-regulated genes, or sequences bound by a factor of interest from a ChIP-seq experiment), then use motif-discovery algorithms that search for short, statistically overrepresented patterns across these sequences relative to an appropriate background model (e.g., overall genomic nucleotide composition), typically using either exhaustive enumeration for very short motifs or probabilistic/expectation-maximization-based approaches for longer or more degenerate motifs. Statistical significance assessment is critical here — with enough sequences and short enough motif lengths, spurious "motifs" can appear to be enriched purely by chance, so appropriate background models and multiple-testing-aware significance thresholds are essential rather than optional.

**Follow-ups:**
- Why is choosing an appropriate background/null model so important for motif discovery, and what could go wrong if you used a naive uniform-nucleotide-frequency background instead of a more realistic one?

---

### Q: What is molecular clock analysis, and what key assumption does it rely on that isn't always biologically true? 🔴

**Answer:**
Molecular clock analysis uses the observed number of sequence differences (substitutions) between species/lineages, combined with an assumed or calibrated rate of molecular evolution, to estimate the timing of evolutionary divergence events — essentially treating the accumulation of mutations over time as a rough "clock" that can be used to date evolutionary splits, often calibrated against known fossil or geological dates for at least some points in the tree.

The key assumption (the "molecular clock hypothesis" in its strict form) is that the rate of molecular evolution is roughly constant across different lineages and over time — an assumption that's frequently violated in practice, since mutation/substitution rates can vary substantially between lineages due to differences in generation time, population size, selective pressures, DNA repair efficiency, and other biological factors. This is why modern molecular clock analyses commonly use **relaxed molecular clock models**, which explicitly allow substitution rates to vary across branches of the tree (often modeled statistically, e.g., as drawn from a distribution across lineages) rather than assuming a single strict, constant rate — producing generally more realistic and defensible divergence time estimates, especially across evolutionarily distant or biologically heterogeneous lineages.

**Follow-ups:**
- Why might using a strict molecular clock model on a dataset spanning organisms with very different generation times (e.g., comparing a short-generation insect lineage to a long-generation mammal lineage) produce systematically biased divergence time estimates?
