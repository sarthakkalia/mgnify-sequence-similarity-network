# MGnify Sequence Similarity Network (SSN) Explorer

A prototype pipeline for constructing and analyzing **Sequence Similarity Networks (SSNs)**
using protein sequences from the [MGnify Proteins database](https://www.ebi.ac.uk/metagenomics/proteins).

Developed as part of the preparation for the **Google Summer of Code (GSoC) project:**
> *"Sequence Similarity Networks for the visualisation and exploration of MGnify Proteins"* — EMBL-EBI

---

## What is a Sequence Similarity Network?

An SSN is a graph where:
- **Nodes** represent protein sequences
- **Edges** represent similarity between pairs of proteins

SSNs are used to explore protein families, identify functional clusters, and study
evolutionary relationships across large metagenomic datasets.

---

## Repository Structure

```
mgnify-sequence-similarity-network/
│
├── notebooks/
│   ├── 1st-prototype.ipynb       # Alignment-based SSN using MMseqs2
│   └── 2nd-prototype.ipynb       # Embedding-based SSN (ESM2) + Hybrid SSN
│
├── data/                         # Input protein sequences and biome metadata
│
├── results/                      # Generated node and edge files (CSV)
│
├── visualization/                # Cosmograph-compatible export files
│
└── README.md
```

---

## Notebooks

### `1st-prototype.ipynb` — Alignment-Based SSN
Constructs an SSN using **pairwise sequence alignment similarity** computed via MMseqs2.

Pipeline:
1. Load protein sequences from FASTA files
2. Run MMseqs2 pairwise similarity search
3. Filter edges using a similarity threshold (0.4)
4. Build graph with NetworkX
5. Annotate nodes with MGnify biome metadata
6. Apply Louvain community detection
7. Export for visualization (Cosmograph / Cytoscape)

**Results on 10,000 MGnify protein sequences:**

| Metric | Value |
|---|---|
| Nodes | 10,000 |
| Edges | ~18,600 |
| Similarity threshold | 0.4 |
| Communities detected | ~6,700 |

🔗 **[View Interactive Visualization (Cosmograph)](https://run.cosmograph.app/public/b846ab38-b25d-420b-9d53-cf454641fc42)**

---

### `2nd-prototype.ipynb` — Embedding-Based SSN + Hybrid SSN
Extends the baseline with two additional similarity approaches.

---

#### Part A — Embedding-Based SSN
Constructs an SSN using **protein language model embeddings** from ESM2 (Meta AI).

Pipeline:
1. Load protein sequences
2. Generate dense vector embeddings using ESM2
3. Compute cosine similarity between embedding pairs
4. Filter edges using a similarity threshold (0.8)
5. Build and analyze the embedding-based graph

**Results on 10,000 MGnify protein sequences:**

| Metric | Value |
|---|---|
| Nodes | 10,000 |
| Edges | ~29,590,000 |
| Similarity threshold | 0.8 |
| Communities detected | ~6,700 |

> **Key observation:** Embedding-based similarity produces significantly denser graphs
> than alignment-based methods, highlighting the need for sparsification strategies.

---

#### Part B — Hybrid SSN
Combines **alignment-based** and **embedding-based** similarity into a single weighted score:

```
S_hybrid = α · S_align + β · S_embed
```

where `α = β = 0.5` assigns equal weight to both similarity signals. This balances
the high specificity of alignment-based edges with the broader coverage of
embedding-based relationships.

Pipeline:
1. Compute alignment similarity (MMseqs2) and embedding similarity (ESM2) separately
2. Combine scores using the weighted hybrid formula
3. Filter edges using a combined threshold (0.5)
4. Build hybrid graph with NetworkX
5. Annotate nodes with MGnify biome metadata
6. Apply Louvain community detection

**Results on 10,000 MGnify protein sequences:**

| Metric | Value |
|---|---|
| Nodes | 10,000 |
| Edges | ~25,000 |
| Threshold (α, β) | 0.5, 0.5 |
| Communities detected | ~5,600 |

> **Key observation:** The hybrid SSN achieves better connectivity than the alignment-only
> network while remaining far more interpretable than the dense embedding-only graph.

🔗 **[View Interactive Visualization (Cosmograph)](https://run.cosmograph.app/project/86a6ac5b-fb80-4338-bcea-ff4bc94daec9)**

---

## Comparison of SSN Approaches

| Approach | Edges | Communities | Characteristics |
|---|---|---|---|
| Alignment-based (MMseqs2) | ~18,600 | ~6,700 | High specificity, sparse |
| Embedding-based (ESM2) | ~29,590,000 | ~6,700 | High coverage, extremely dense |
| Hybrid (alignment + embedding) | ~25,000 | ~5,600 | Balanced — interpretable and connected |

---

## Technologies Used

| Tool | Purpose |
|---|---|
| MMseqs2 | Fast pairwise sequence alignment |
| ESM2 (Meta AI) | Protein language model embeddings |
| NetworkX | Graph construction and analysis |
| python-louvain | Community detection |
| Biopython | FASTA parsing |
| Cosmograph | Large-scale graph visualization |
| Cytoscape | Interactive network exploration |

---

## Dataset

- **Source:** [MGnify Proteins database](https://www.ebi.ac.uk/metagenomics/proteins)
- **Subset used:** 10,000 protein sequences
- **Annotations:** Biome metadata per protein node

---

## Next Steps

- Similarity thresholding and node filtering to control graph density further
- Hadamard product similarity as an alternative to cosine similarity
- Biome-aware cluster analysis and enrichment evaluation
- Tuning of α and β weights based on cluster quality metrics
- Scaling to larger MGnify subsets (100K–1M proteins)

---

## Author

**Sarthak Kalia**  
B.Tech Computer Science & Engineering (AI & ML), Sambalpur University <br>
AI/ML Engineer <br>
GitHub: [@sarthakkalia](https://github.com/sarthakkalia)
