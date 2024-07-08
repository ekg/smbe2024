---
title: Implicit Pangenome Graphs
subtitle: Scaling Comparative Genomics Beyond the Phylogenetic Tree
author: Erik Garrison
date: SMBE 2024
---

# What are Pangenome Graphs?

- Model full genomic information of a species or clade
- Represent mutual relationships between all genomes
- Encode DNA sequences as walks through a sequence graph

---

# Limitations of Current Methods

- Reference bias in traditional approaches
  - Analyses limited to sequences similar to chosen reference
- Challenges with tree-based methods
  - Dependent on specific ordering of genomes
  - Guide tree topology affects results

---

# The Need for Unbiased Pangenome Graphs

- Represent alignment of all genomes to all others
- Avoid limitations of:
  - Single reference genomes
  - Progressive alignment approaches
  - Fixed k-mer length de Bruijn graphs

---

# Existing Methods: Cactus

- Based on phylogenetic trees
- Provides scalability for multiple genome alignment
- Limitations:
  - Changing included genomes affects aligned regions
  - Guide tree biases Incomplete Lineage Sorting (ILS) detection

---

# Direct Graph Creation Problems

- Computationally intensive ("graphs are STICKY")
- High memory requirements for loading entire graph
- Real-world example:
  - 35-day attempt with 16 T2T great ape genomes
  - Ended in failure due to power outage

---

# Need for a Scalable, Unbiased Approach

- Handle tens to thousands of genomes
- Avoid reference bias and input order dependency
- Allow efficient updates with new genomes
- Operate on commodity hardware

---

# Concept of Implicit Data Structures

- Simulate full data structure without instantiating it
- Provide interface to query as if full structure exists
- Example: Implicit interval tree
  - Used in bedtools and alignment algorithms
  - Efficient for range overlap queries

---

# Benefits of Implicit Data Structures

- Memory efficiency
  - Avoid storing entire structure in memory
- Scalability to large datasets
  - Can handle data larger than available RAM
- Fast query operations
  - Often logarithmic time complexity

---

# Core Idea of Implicit Pangenome Graphs

- Graph implied by set of pairwise alignments
- Never materialize the full graph structure
- Dynamically compute graph properties on-the-fly

---

# Key Components of Implicit Pangenome Graphs

1. Intervals as alignment ranges over target references
   - Efficient representation of matches between sequences
2. Coordinate translation as the fundamental operation
   - Projecting coordinates between different genomes

---

# Mathematical Foundation

- Transitive closure of matches
  - m+ = {i...j}, set of characters transitively linked by matches
- Union-find operations for match closures
  - Efficiently compute transitive closures

---

# Advantages of Implicit Pangenome Graphs

- Reduced memory requirements
  - Only store alignments, not full graph structure
- Scalability to large numbers of genomes
  - Can handle hundreds or thousands of genomes
- Flexibility in representation of genomic relationships
  - Easy to update with new genomes or alignments

---

# impg Tool Architecture

- Input: bgzip-compressed PAF file of alignments
- Output: BED, BEDPE, PAF formats
- Core components:
  - Sequence indexing (SequenceIndex)
  - Alignment processing
  - Coordinate translation

---

# Key Data Structures in impg

- BasicCOITree (Compact Overlapping Interval Tree)
  - Efficient range queries on alignments
- SequenceIndex
  - Maps between sequence names and numeric IDs
  - Stores sequence lengths
- Disk-backed data structures
  - Efficient handling of large datasets

---

# Core Operations in impg

1. Stabbing query on the BasicCOITree
   - Find overlapping alignments efficiently
2. CIGAR string traversal for position-level translation
   - Dynamic parsing of CIGARs for coordinate projection

---

# Transitive Collection in impg

- Algorithm for collecting all mutually aligned sequences
- Steps:
  1. Initial query for overlapping alignments
  2. Recursive collection of transitively aligned sequences
  3. Projection of coordinates through alignments

---

# Optimizations in impg

- Match compression
  - Reduce memory usage by encoding runs of matches
- Partitioning for parallel processing
  - Divide work into chunks for multi-threaded execution
- Disk-backed data structures
  - Use external memory to handle large datasets

---

# Comparison of impg to seqwish

- Similarities:
  - Both work with pairwise alignments
  - Use transitive closures of matches
- Differences:
  - seqwish materializes the graph
  - impg keeps the graph implicit

---

# T2T Primate Project Overview

- All-vs-all alignment of primate genomes
  - Includes human, chimpanzee, bonobo, gorilla, orangutan
- Incorporation of HPRC year 1 human genome assemblies
- Goal: Create a comprehensive primate pangenome resource

---

# Pangenome Alignment Process

1. Iterative all-vs-one approach using wfmash
   - MashMap3 for initial mapping
   - Filtering and merging of mappings
2. Base-level alignment with wavefront algorithm
3. Conversion to CHAIN format using paf2chain

---

# Subgraph Analysis: MHC Locus

- Extraction process using impg
  - Command: `impg -p primates16.20231205_wfmash-v0.12.5/chm13#1.aln.paf -q chm13#1#chr6:28385000-33300000`
- Visualization and analysis of structural variants
  - Identified C4A/B locus with endogenous retrovirus
  - High structural variation in MHC class II region

---

# Subgraph Analysis: 8p23.1 Inversion

- Defensin gene content and polymorphism
- Graph construction using pggb
  - Parameters: `-c 10 -t 96 -p 90 -s 10k -k 19`
- Visualization with odgi
  - Showed relationship between graph topology, inversion, and copy number variation

---

# SNP vs. Gap Divergence Analysis

- Computation method:
  - 1MB segments across target haplotype
  - SNV and gap divergence estimated
- Results for human-NHP comparisons:
  - Lowest SNV divergence: human-chimpanzee/bonobo (0.15-0.16%)
  - Highest gap divergence: human-gorilla (17.9-27.3%)
- Within-species autosome divergence trends

---

# Conservation Analyses

- PhastCons approach:
  1. Default parameters (unsupervised EM learning)
  2. 4-fold degenerate site model
- Distribution of conservation scores
  - Strongly skewed towards 1 (mean 0.964)
- Gene conservation analysis
  - Highest: retained introns, snoRNAs, protein-coding genes
  - Lowest: various classes of pseudogenes
- Comparison with probability of loss intolerance (pLI)
  - Strong correlation (P=8e-164)

---

# Current Limitations and Future Work

- Implementation focuses on range queries, not string matching
- FASTA sequence extraction not currently implemented
- Potential for improved handling of different alignment formats
- Development of a total subdivising algorithm
  - Goal: Enable distributed construction of large comparative genomic pangenome graphs
- Challenges:
  - Maintaining consistency across distributed components
  - Efficient merging of partial graphs

---

# Potential Applications

- Analysis of structural variants across species
  - Identify conserved and species-specific SVs
- Evolutionary studies and ancestral genome reconstruction
  - Infer ancestral sequences and genomic rearrangements
- Comparative functional genomics
  - Study conservation of regulatory elements across species

---

# Integration with Other Tools

- Combination with multiple sequence alignment tools
  - Refine alignments in complex regions
- Development of specialized visualization techniques
  - Interactive exploration of large-scale pangenome graphs
- Integration with variant calling and genotyping methods
  - Leverage pangenome information for improved accuracy

---

# Scaling to Larger Datasets

- Strategies for handling thousands of genomes
  - Improved partitioning and distributed processing
  - Adaptive sampling of alignments for initial graph construction
- Potential for cloud-based distributed processing
  - Leverage cloud computing for large-scale analyses
- Optimization of core algorithms for big data

---

# Recap of Implicit Pangenome Graphs

- Key advantages:
  - Scalability to large numbers of genomes
  - Flexibility in representation and updates
  - Unbiased representation of genomic relationships
- Overcomes limitations of traditional approaches
- Efficient range queries using BasicCOITree

---

# Impact on Comparative Genomics

- Revolutionizing analysis of genomic relationships
  - Enables unbiased, large-scale comparisons
- Facilitating evolutionary studies across multiple species
- Potential for new insights into genome function and evolution

---

# Call for Collaboration

- Open-source project: https://github.com/ekg/impg
- Opportunities for:
  - Method refinement and optimization
  - Application to diverse biological questions
  - Integration with existing genomics pipelines

---

# Thank You

Questions?