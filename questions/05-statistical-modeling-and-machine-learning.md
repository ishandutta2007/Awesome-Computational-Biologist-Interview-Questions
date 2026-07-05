# 5. Statistical Modeling & Machine Learning

Rigorous statistical thinking applied to biological data — often small-sample, high-dimensional, and noisy in domain-specific ways.

---

### Q: Why is "small n, large p" (few samples, many features) such a persistent and defining challenge in computational biology, and what statistical strategies help address it? 🟡

**Answer:**
Many biological datasets (gene expression studies, GWAS, proteomics) measure a very large number of features (thousands to millions of genes/variants) but have a comparatively small number of biological samples (often tens to low hundreds), due to the real cost and practical difficulty of generating each additional biological sample/replicate. This "small n, large p" regime creates serious statistical challenges: standard statistical models can easily overfit, apparent patterns can arise from chance given so many features tested, and traditional statistical assumptions (e.g., stable variance estimates per feature) become unreliable with so few samples per feature.

Strategies to address this: **regularization techniques** (e.g., ridge or lasso regression, and their extensions) that penalize model complexity and help prevent overfitting when the number of features vastly exceeds the number of samples; **dimensionality reduction** (e.g., PCA) to reduce the effective number of features being modeled; **borrowing statistical strength across features** (empirical Bayes approaches, as used in differential expression tools, which stabilize per-feature variance estimates by leveraging information across all measured features rather than each feature's own limited replicate data in isolation); and **rigorous, appropriately conservative multiple testing correction** (see the genomics-focused discussion of FDR control) to guard against the inflated false-positive risk inherent to testing huge numbers of features with limited per-test statistical power.

**Follow-ups:**
- Why does regularization (e.g., lasso) become especially important, rather than just helpful, in a setting where the number of features vastly exceeds the number of samples?

---

### Q: How would you decide between a simpler, more interpretable statistical model and a more complex machine learning model for a given computational biology problem? 🟡

**Answer:**
Key considerations:
- **Sample size relative to model complexity:** complex models with many parameters (e.g., deep neural networks) generally require substantially more training data to fit reliably without overfitting than simpler models — given the small-sample-size reality of much biological data, a complex model isn't automatically justified just because it's more flexible in principle.
- **Interpretability requirements:** many computational biology applications (e.g., understanding which specific genetic or molecular features drive a biological outcome) place real value on interpretability, since the scientific goal is often to generate a testable biological hypothesis about mechanism, not just to produce an accurate but opaque black-box prediction — a simpler, more interpretable model that clearly identifies which features matter is often more scientifically useful than a marginally more accurate but uninterpretable one.
- **Validated performance gain:** any added model complexity should be justified by a genuine, rigorously validated performance improvement on held-out or, ideally, truly independent/prospective data — not just better performance on the same data used for model development, which can reflect overfitting rather than genuine improvement.
- **What the downstream use case actually requires:** a purely predictive screening tool (where you mainly care about accurate ranking/classification) has different requirements than a tool meant to generate mechanistic biological insight — the right model choice depends on which of these is the actual goal.

**Follow-ups:**
- Give an example of a computational biology problem where you'd strongly prefer an interpretable model even at some cost to raw predictive accuracy, and explain why.

---

### Q: What is cross-validation, and what special considerations apply when doing cross-validation on biological data with correlated or hierarchical structure (e.g., multiple samples from the same patient, or samples from the same experimental batch)? 🔴

**Answer:**
Cross-validation estimates a model's likely performance on new, unseen data by repeatedly splitting the available data into training and validation subsets, training on one subset and evaluating on the held-out subset, and aggregating performance across multiple such splits — used both for model evaluation and for tuning model hyperparameters.

Special considerations for biological data with correlated/hierarchical structure: **naive random cross-validation splitting can produce misleadingly optimistic performance estimates** if, for example, multiple samples from the same patient (or the same experimental batch) end up split across both the training and validation sets — the model can then appear to generalize well partly because it's implicitly "cheating" by learning patient-specific or batch-specific characteristics from the training set that then trivially help it perform well on the same patient's/batch's held-out samples, rather than genuinely learning to generalize to new, independent patients/batches. The appropriate fix is **grouped cross-validation** (ensuring all samples from the same patient, batch, or other relevant grouping unit are kept together, entirely within either the training or the validation set for each fold) — this is directly analogous to the scaffold-splitting concern for chemical data discussed in the AI Drug Discovery Scientist companion repo, and reflects a general principle: cross-validation splits should reflect the actual independence structure relevant to how the model will be used in practice.

**Follow-ups:**
- If you inherited an analysis pipeline that used naive random cross-validation on a dataset with multiple samples per patient, and it reported impressively high accuracy, how would you investigate whether this estimate is trustworthy?

---

### Q: What is the difference between a p-value and an effect size, and why do computational biology papers increasingly emphasize reporting both rather than p-values alone? 🟡

**Answer:**
A p-value indicates the probability of observing a result at least as extreme as the one obtained, under the assumption that the null hypothesis (no real effect) is true — it speaks to statistical significance (how likely the observed pattern is to have arisen by chance alone) but says nothing directly about the magnitude or biological/practical importance of the effect. Effect size (e.g., fold-change, correlation coefficient, odds ratio) directly quantifies the magnitude of an observed relationship or difference, independent of sample size.

Emphasizing both matters because with a large enough sample size, even a biologically trivial, negligibly small effect can achieve high statistical significance (a very small p-value) — meaning a significant p-value alone doesn't tell you whether an effect is biologically meaningful or actionable. Conversely, a study with a small sample size might fail to reach statistical significance for an effect that is actually biologically substantial, simply due to insufficient statistical power — reporting effect size alongside the p-value (and ideally a confidence interval around the effect size estimate) gives a much more complete and honest picture of both the statistical reliability and the practical/biological magnitude of a finding.

**Follow-ups:**
- Describe a scenario where a computational biology result could be statistically highly significant but have little to no real biological importance.

---

### Q: How would you validate that a predictive machine learning model built on biological data will actually generalize, beyond just checking held-out performance on your original dataset? 🔴

**Answer:**
- **External/independent validation:** test the model on a genuinely independent dataset — ideally generated by a different lab, using different experimental protocols or platforms, and preferably collected after the original model development — since a model that performs well only on data very similar to its own training distribution (even with proper internal cross-validation) may still fail badly on real-world independent data with different technical or population characteristics.
- **Check for and account for confounding/batch structure**, as discussed above and in the genomics-focused companion repo, since apparent generalization within a single dataset can be inflated by shared technical artifacts across the "independent" splits if not properly controlled.
- **Assess whether the training data adequately represents the population/context the model will actually be applied to** — a model trained on a specific demographic, disease subtype, or experimental condition may not generalize to meaningfully different contexts, and this should be explicitly checked rather than assumed away.
- **Where feasible, pursue prospective validation** — testing the model's predictions against new data/experiments generated specifically to test it, which is the most rigorous (though slowest and most resource-intensive) form of validation, directly analogous to the retrospective-versus-prospective validation distinction discussed in the AI Drug Discovery Scientist companion repo.

**Follow-ups:**
- Your model performs excellently on cross-validation within your original dataset but noticeably worse on an independent dataset from a different lab. What would you investigate first?
