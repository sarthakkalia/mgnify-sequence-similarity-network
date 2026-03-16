# MGnify Sequence Similarity Network Explorer

Prototype pipeline for constructing and analyzing **Sequence Similarity Networks (SSN)** for proteins from the **MGnify Proteins database**.

This project was developed as an exploratory prototype for the **Google Summer of Code (GSoC) project:**

> *“Sequence Similarity Networks for the visualisation and exploration of MGnify Proteins”*

The goal is to construct a graph representation of protein similarity using **MMseqs2**, analyze graph communities, and visualize the resulting network.

---

# Project Overview

Protein datasets from metagenomics studies can contain **millions of sequences**.  
Sequence Similarity Networks (SSNs) provide a **graph-based way to explore relationships between proteins**.

In an SSN:

- **Nodes** represent proteins  
- **Edges** represent sequence similarity relationships  

Graph analysis can reveal:

- Protein families  
- Functional clusters  
- Cross-biome relationships  

---

# Prototype Pipeline

The current prototype implements the following workflow:

```

Protein sequences
↓
MMseqs2 pairwise similarity search
↓
Similarity score filtering using defined threshold
↓
Sequence Similarity Network (NetworkX)
↓
Biome metadata annotation
↓
Community detection (Louvain clustering)
↓
Cluster analysis
↓
Network visualization (Cosmograph / Cytoscape)

```

---

# Dataset Used

Subset of proteins from the **MGnify Proteins database**

```

Proteins: 10,000
Edges: 18,644
Similarity threshold: 0.4
Communities detected: 6,736

```

Each protein node is annotated with **biome metadata**.

---

# Technologies Used

- Python
- MMseqs2
- NetworkX
- Pandas
- Biopython
- Louvain Community Detection
- Cytoscape
- Cosmograph

---

# Repository Structure

```

mgnify-ssn-explorer
│
├── data/
│   ├── subset.fasta
│   └── metadata.tsv
│
├── notebooks/
│   └── 1st-proposal.ipynb
│
│
├── results/
│   ├── nodes.csv
│   └── edges.csv
│
├── visualization/
│   ├── textFile_cosmograph.txt
│
└── README.md

```

---

# Example Network Output

Example SSN statistics from the prototype:

```

Nodes: 10,000
Edges: 18,644
Communities: 6,736

```

Clusters correspond to potential **protein families or similarity groups**.

---

# Visualization

Networks can be visualized using:

- **NetworkX** — basic graph visualization  
- **Cytoscape** — interactive network exploration  
- **Cosmograph** — large-scale graph visualization  

## Example Visualization

![SSN Graph]([visualization/Cosmograph_Untitled project_2026-03-12_01-23 (1).png](https://github.com/sarthakkalia/mgnify-sequence-similarity-network/blob/main/visualization/Cosmograph_Untitled%20project_2026-03-12_01-23%20(1).png))

```

nodes → proteins
edges → similarity relationships
node color → biome
node grouping → community cluster

```

---

# Future Improvements

Potential extensions for the project include:

- Multi-threshold SSN construction  
- Protein embedding-based similarity (ESM models)  
- Hybrid similarity networks (alignment + embeddings)  
- Graph neural network analysis of SSNs  
- Scalable SSN generation for larger MGnify datasets  

---

# Author

**Sarthak Kalia**  
AI/ML Student
