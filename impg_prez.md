---
title: Implicit Pangenomics
author: Erik Garrison
date: SMBE 2024
---

# Introduction to Pangenomics

- Pangenome: entire genetic content of a species or population
- Captures genetic diversity beyond a single reference genome
- Crucial for understanding evolutionary processes and population dynamics
- Challenges: 
  - Representing complex structural variations
  - Scaling to large numbers of diverse genomes

---

# Variation Graphs in Comparative Genomics

- Graph-based structures representing genetic variation
- Nodes: sequence fragments; Edges: connections between fragments
- Benefits:
  - Efficient representation of complex genomic regions
  - Capture evolutionary relationships between sequences
- Applications:
  - Alignment of diverse genomes
  - Variant calling and genotyping across species

---

# Current Approaches in Human Pangenomics

- Human Pangenome Reference Consortium (HPRC) work:
  - Goal: Comprehensive representation of human genetic diversity
  - Method: Variation graphs partitioned by reference chromosomes
- Challenges:
  - Memory and time requirements for graph creation
  - Balancing comprehensiveness with practical usability
  - Integrating data from diverse populations effectively

---

# Limitations of Current Methods

- Reference bias in traditional approaches
- Computational intensity of direct graph creation
- Scalability issues with increasing numbers of genomes
- Example: 35-day failed attempt with 16 T2T great ape genomes
- Need for unbiased, scalable approach for comparative genomics

---

# Implicit Data Structures: A Solution

- Simulate full data structure without materializing it
- Provide interface to query as if full structure exists
- Example: Implicit interval tree
  - Efficient for range overlap queries
  - Used in bedtools and alignment algorithms
- Benefits: Memory efficiency, scalability, fast query operations

---

# Core Concept of Implicit Pangenome Graphs

- Graph implied by set of pairwise alignments
- Never materialize the full graph structure
- Dynamically compute graph properties on-the-fly
- Key components:
  1. Intervals as alignment ranges over target references
  2. Coordinate translation as fundamental operation

---

# Mathematical Foundation

- Transitive closure of matches:
  - m+ = {i...j}, set of characters transitively linked by matches
- Union-find operations for match closures
- Efficient computation of transitive relationships
- Enables representation of complex evolutionary relationships

---

# Advantages of Implicit Pangenome Graphs

- Reduced memory requirements
- Scalability to large numbers of genomes
- Flexibility in representation and updates
- Unbiased representation of genomic relationships
- Efficient for comparative genomics across diverse species

---

# impg: Implementation Architecture

- Input: bgzip-compressed PAF file of alignments
- Output: BED, BEDPE, PAF formats
- Core components:
  - Sequence indexing (SequenceIndex)
  - Alignment processing
  - Coordinate translation
- Key data structures:
  - BasicCOITree (Compact Overlapping Interval Tree)
  - Disk-backed data structures for large-scale genomic data

---

# Core Operations in impg

1. Stabbing query on the BasicCOITree
   - Efficiently find overlapping alignments across genomes
2. CIGAR string traversal for position-level translation
   - Dynamic parsing of CIGARs for coordinate projection
3. Transitive collection of aligned sequences
   - Recursive collection and projection of coordinates
- Enables efficient comparison across multiple species or populations

---

# Application: T2T Primate Project

- All-vs-all alignment of primate genomes
  - Human, chimpanzee, bonobo, gorilla, orangutan
- Incorporation of HPRC year 1 human genome assemblies
- Process:
  1. Iterative all-vs-one approach using wfmash
  2. Base-level alignment with wavefront algorithm
  3. Conversion to CHAIN format using paf2chain
- Demonstrates scalability to multiple closely-related species

---

# Subgraph Analysis: MHC Locus

- Extraction using impg:
  `impg -p primates16.20231205_wfmash-v0.12.5/chm13#1.aln.paf -q chm13#1#chr6:28385000-33300000`
- Visualization and analysis of structural variants:
  - C4A/B locus with endogenous retrovirus
  - High structural variation in MHC class II region
- Insights into immune system evolution across primates

---

# Subgraph Analysis: 8p23.1 Inversion

- Defensin gene content and polymorphism
- Graph construction using pggb:
  Parameters: `-c 10 -t 96 -p 90 -s 10k -k 19`
- Visualization with odgi:
  - Relationship between graph topology, inversion, and copy number variation
- Evolutionary implications of structural variations in primates

---

# SNP vs. Gap Divergence Analysis

- Method: 1MB segments across target haplotype
- Results for human-NHP comparisons:
  - Lowest SNV divergence: human-chimpanzee/bonobo (0.15-0.16%)
  - Highest gap divergence: human-gorilla (17.9-27.3%)
- Within-species autosome divergence trends
- Insights into primate genome evolution and speciation

---

# Conservation Analyses

- PhastCons approach:
  1. Default parameters (unsupervised EM learning)
  2. 4-fold degenerate site model
- Distribution of conservation scores (mean 0.964)
- Gene conservation analysis across primates
- Comparison with probability of loss intolerance (pLI)
- Implications for understanding evolutionary constraints

---

# Scaling to Large Datasets

- Strategies for handling thousands of genomes:
  - Improved partitioning and distributed processing
  - Adaptive sampling of alignments
- Potential for cloud-based distributed processing
- Applications in large-scale comparative genomics:
  - Cross-species evolutionary studies
  - Population-level analyses within species

---

# Future Directions in Comparative Pangenomics

- Extending impg to accommodate 100,000+ genomes
- Development of a total subdivising algorithm:
  - Distributed construction of large comparative genomic pangenome graphs
- Integration with large-scale genomic projects across species
- Applications in evolutionary studies and population genetics

---

# Potential Applications

- Analysis of structural variants across species and populations
- Evolutionary studies of genetic diversity
- Comparative functional genomics:
  - Study conservation of regulatory elements across species
- Insights into speciation and adaptation processes

---

# Integration with Existing Tools

- Combination with multiple sequence alignment tools
- Development of specialized visualization techniques
- Integration with variant calling and genotyping methods
- Synergy with existing pangenome construction tools (e.g., PGGB)

---

# Conclusion: Impact on Comparative Genomics

- Scalable, unbiased representation of genomic relationships
- Enables large-scale comparisons across diverse species and populations
- Facilitates evolutionary studies of genetic diversity
- Potential for new insights into genome function and evolution

---

# Call for Collaboration

- Open-source project: https://github.com/ekg/impg
- Opportunities for:
  - Method refinement and optimization
  - Application to diverse evolutionary studies
  - Integration with existing genomics pipelines
- Join us in advancing comparative pangenomics!

---

# Thank You

Questions?