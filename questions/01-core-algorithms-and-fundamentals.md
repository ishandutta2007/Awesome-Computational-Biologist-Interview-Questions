# 1. Core Algorithms & Fundamentals

The algorithmic backbone of computational biology — concepts that show up whether you're aligning sequences, folding proteins, or building phylogenies.

---

### Q: Explain how dynamic programming is used in sequence alignment, and walk through the intuition behind the Needleman-Wunsch algorithm. 🟢

**Answer:**
Dynamic programming solves sequence alignment by breaking the problem of aligning two full sequences into smaller subproblems (aligning prefixes of each sequence) and building up a solution using a scoring matrix, where each cell represents the optimal alignment score for the corresponding prefixes of the two sequences.

Needleman-Wunsch (global alignment) fills this matrix using a recurrence: the score at each cell is the maximum of (a) extending a match/mismatch from the diagonal cell, (b) introducing a gap in one sequence (moving from the cell above), or (c) introducing a gap in the other sequence (moving from the cell to the left) — each option incorporating an appropriate match/mismatch or gap penalty score. Once the matrix is filled, a traceback step reconstructs the actual optimal alignment by following the path of choices that led to the final, optimal score.

This is a classic example of dynamic programming's core principle: the optimal solution to the full problem can be constructed from optimal solutions to its subproblems, avoiding redundant recomputation by storing (memoizing) subproblem results in the matrix.

**Follow-ups:**
- How does Smith-Waterman (local alignment) differ from Needleman-Wunsch, and when would you use local versus global alignment?

---

### Q: Why can't tools like BLAST perform an exhaustive dynamic programming alignment against an entire large sequence database, and what heuristic approach do they use instead? 🟡

**Answer:**
Exhaustive dynamic programming alignment (as in Needleman-Wunsch/Smith-Waterman) has time complexity proportional to the product of the two sequences' lengths — computationally fine for aligning two individual sequences, but far too slow to run exhaustively against every sequence in a large database (which could contain millions of sequences) for routine, fast search use cases.

BLAST (Basic Local Alignment Search Tool) uses a heuristic seed-and-extend approach instead: it first breaks the query sequence into short subsequences (words/k-mers) and rapidly finds exact or near-exact matches ("seeds") of these short words in the database using an efficient lookup structure, then extends only these promising seed matches into full local alignments using a faster approximate scoring approach, rather than running a full dynamic programming alignment against every database sequence from scratch. This trades a small amount of sensitivity (it can occasionally miss a true, weak alignment with no strong short seed match) for a massive gain in speed, which is why BLAST is a heuristic, not a guaranteed-optimal algorithm.

**Follow-ups:**
- In what scenario might BLAST's heuristic approach miss a biologically real, significant alignment that an exhaustive method would have found?

---

### Q: What is time and space complexity, and why does this matter practically when choosing or designing an algorithm for a genome-scale computational biology problem? 🟢

**Answer:**
Time complexity describes how an algorithm's runtime scales with input size, and space complexity describes how memory usage scales with input size — both typically expressed using big-O notation to characterize the growth rate (e.g., linear, quadratic, exponential) rather than an exact runtime.

This matters enormously in computational biology because inputs are often extremely large (a human genome is roughly 3 billion base pairs, and many analyses need to scale to many samples or entire databases) — an algorithm with quadratic time complexity that's perfectly fine for a small test case can become computationally infeasible at genome scale, and an algorithm using memory proportional to the square of sequence length would exceed available memory entirely for full chromosomes. This is precisely why practical genome-scale tools rely on heuristics, indexing structures (e.g., suffix arrays, Burrows-Wheeler transform-based indices used in modern short-read aligners), and approximate algorithms rather than naive exhaustive approaches, even when the exhaustive approach would technically give a more "exact" answer.

**Follow-ups:**
- Why do modern short-read aligners typically use a Burrows-Wheeler transform-based index rather than a naive string-matching approach, and what tradeoff does this involve?

---

### Q: What is a suffix array (or suffix tree), and what class of biological sequence problems does it help solve efficiently? 🔴

**Answer:**
A suffix array is a data structure that stores all suffixes of a string in sorted (lexicographic) order, typically represented as an array of starting indices rather than storing the actual substrings themselves (which would be memory-prohibitive for large sequences) — it allows efficient, fast substring search (e.g., binary search) within a large reference sequence, avoiding the need for a linear scan.

This is foundational for problems like short-read alignment (efficiently finding where a short read matches within a large reference genome), genome assembly, and repeat detection — precomputing this index structure once for a reference genome allows very fast repeated queries against it, which is essential when you need to align millions of reads (or search for many different query sequences) against the same reference. A suffix tree is a related but more memory-intensive structure representing the same information in a compressed trie form; in practice, modern tools often use more memory-efficient derived structures (e.g., the FM-index, built using the Burrows-Wheeler transform) that provide similar fast-search capability with a much smaller memory footprint, which is the practical reason most production short-read aligners are built around this approach rather than a naive suffix array or suffix tree.

**Follow-ups:**
- Why is memory footprint such a critical design constraint for genome-scale indexing structures, and how does the Burrows-Wheeler transform help address it?

---

### Q: What is a hidden Markov model (HMM), and give an example of a computational biology problem where HMMs are a natural fit. 🟡

**Answer:**
A hidden Markov model describes a system as a sequence of hidden (unobserved) states, where each state emits an observable symbol according to some probability distribution, and transitions between hidden states follow their own probabilities (typically depending only on the current state — the Markov property). Given a sequence of observations, HMM algorithms (e.g., the Viterbi algorithm) can infer the most likely underlying sequence of hidden states.

A natural fit example: **gene prediction/gene finding**, where the hidden states represent biological features (exon, intron, intergenic region) and the observed sequence is the actual DNA sequence — an HMM-based gene finder infers the most likely underlying gene structure given the observed nucleotide sequence, based on learned emission probabilities (which nucleotide patterns are typical of exons versus introns) and transition probabilities (how gene structure elements typically follow each other). Other classic applications include protein family/domain classification (profile HMMs) and sequence alignment with more sophisticated gap/state modeling than simple dynamic programming scoring schemes.

**Follow-ups:**
- What is a profile HMM, and how does it differ from a standard HMM in how it's typically constructed and used?

---

### Q: What is the difference between a greedy algorithm and dynamic programming, and can you give a computational biology example where a greedy approach would give a suboptimal (wrong) answer? 🟡

**Answer:**
A greedy algorithm makes the locally optimal choice at each step without reconsidering previous decisions, hoping (without guarantee) that this leads to a globally optimal solution — fast and simple, but only correct for problems with the specific mathematical property that local optimality actually guarantees global optimality (the "greedy choice property"). Dynamic programming instead systematically considers all relevant subproblem solutions and combines them to guarantee finding the true global optimum, at the cost of more computation and memory.

A computational biology example where greedy fails: **naive sequence alignment via a simple left-to-right, locally-best-choice matching approach** (e.g., always matching the next characters if they're identical, without considering that introducing an early gap might allow a much better overall alignment later) would generally produce a suboptimal alignment compared to the guaranteed-optimal alignment found by full dynamic programming (Needleman-Wunsch/Smith-Waterman) — this is precisely why alignment tools use dynamic programming rather than a simpler greedy heuristic when an exact, provably optimal alignment is required for the specific sequence lengths involved.

**Follow-ups:**
- Genome assembly algorithms sometimes use greedy heuristics (e.g., greedily merging the most similar-looking reads/contigs first) despite the risk of suboptimality. Why might this tradeoff be acceptable in that context?
