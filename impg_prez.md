---
title: Implicit Pangenomics
subtitle: Scaling Base-Level Analyses Across the Vertebrate Clade
author: Erik Garrison
date: SMBE 2024
---

# Introduction to Pangenomes

- Pangenome: the entire genetic content of a species or population
- Captures genetic diversity beyond a single reference genome
- Crucial for understanding:
  - Population diversity
  - Evolutionary processes
  - Genome function and adaptation

---

# Pangenome Graphs

- Graphical representation of pangenomes
- Nodes: sequence fragments
- Edges: connections between fragments
- Benefits:
  - Efficient representation of genetic variation
  - Capture complex structural variants
  - Enable comparative genomic analyses

---

# The Challenge in Pangenome Analysis

- Need to scale base-level pangenome analyses to entire vertebrate clade
- Current methods struggle with large numbers of genomes
- Goal: Unbiased, scalable approach for comparative genomics

---

# Limitations of Existing Approaches

1. Progressive pangenome alignment:
   - Biased by order of genome addition
   - Difficult to update with new genomes

2. Tree-guided methods (e.g., Cactus):
   - Limited by phylogenetic assumptions
   - May miss complex evolutionary relationships

3. Direct graph creation:
   - Computationally intensive ("graphs are STICKY")
   - Example: 35-day failed attempt with 16 T2T great ape genomes

---

# The Promise of All-to-All Alignment

- Capture all pairwise relationships without bias
- Enable detection of complex evolutionary events
- Challenges:
  - Computational intensity
  - Need for efficient representation and query

---

# Introducing the Concept of Implicit Data Structures

- Simulate full data structure without materializing it
- Provide interface to query as if full structure exists
- Example: Implicit interval tree
  - Efficient for range overlap queries
  - Used in bedtools and alignment algorithms

---

# What is an Implicit Graph?

- A graph defined by its query interface, not its structure
- Never materialized in full
- Properties computed on-the-fly

---

# Components of an Implicit Pangenome Graph

1. Set of sequences (genomes)
2. Set of alignments between sequences
3. Query interface for:
   - Coordinate projection between genomes
   - Transitive closure of alignments

---

# Building the Implicit Pangenome Graph

1. Start with pairwise alignments between genomes
2. Transform alignments into an implicit interval tree
3. Implement query operations:
   - Find overlapping alignments
   - Project coordinates through alignments
   - Compute transitive closures

---

# Key Data Structure: Implicit Interval Tree

- Efficient representation of alignment intervals
- Allows fast overlap queries
- Crucial for scalable coordinate projection

---

# Mathematical Foundation

- Transitive closure of matches:
  m+ = {i...j}, set of characters transitively linked by matches
- Union-find operations for match closures
- Enables representation of complex evolutionary relationships

---

# impg: Implementation of Implicit Pangenome Graphs

- Input: bgzip-compressed PAF file of alignments
- Output: BED, BEDPE, PAF formats
- Core components:
  - Sequence indexing
  - Alignment processing
  - Coordinate translation

---

# Core Operations in impg

1. Stabbing query on the implicit interval tree
2. CIGAR string traversal for position-level translation
3. Transitive collection of aligned sequences

---

# Application: Vertebrate Genomes Project (VGP)

- Goal: All-to-all alignment of ~500 vertebrate genomes
- Demonstrates scalability of implicit pangenome approach
- Enables comparative genomics across diverse species

---

# Case Study: T2T Primate Project

- All-vs-all alignment of primate genomes
  - Human, chimpanzee, bonobo, gorilla, orangutan
- Process:
  1. Iterative all-vs-one approach using wfmash
  2. Base-level alignment with wavefront algorithm
  3. Conversion to CHAIN format using paf2chain

---

# Subgraph Analysis Examples

1. MHC Locus:
   - High structural variation in MHC class II region

2. 8p23.1 Inversion:
   - Defensin gene content and polymorphism

3. SNP vs. Gap Divergence Analysis:
   - Insights into primate genome evolution

---

# The Human Case: Understanding Ectopic Recombination

- Goal: All-to-all alignment of entire human pangenome
- Importance:
  - Detect ectopic recombination throughout the genome
  - Observed in HPRC between acrocentric chromosomes
- Implications for understanding genome stability and evolution

---

# Scaling to Larger Datasets

- Strategies for handling thousands of genomes:
  - Improved partitioning and distributed processing
  - Adaptive sampling of alignments
- Potential for cloud-based distributed processing

---

# Future Directions

- Extending impg to accommodate 100,000+ genomes
- Development of a total subdivising algorithm
- Integration with large-scale genomic projects
- Applications in evolutionary studies and population genetics

---

# Potential Applications in Comparative Genomics

- Analysis of structural variants across species
- Evolutionary studies of genetic diversity
- Comparative functional genomics
- Insights into speciation and adaptation processes

---

# Conclusion: Impact on Comparative Genomics

- Scalable, unbiased representation of genomic relationships
- Enables base-level analyses across the vertebrate clade
- Facilitates detection of complex evolutionary events
- Potential for new insights into genome function and evolution

---

# Call for Collaboration

- Open-source project: https://github.com/ekg/impg
- Opportunities for method refinement and application

---

# Thank You

Questions?