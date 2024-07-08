---
title: Implicit Pangenome Graphs
subtitle: Scaling Comparative Genomics Beyond the Phylogenetic Tree
author: Erik Garrison
date: SMBE 2024
---

# 1. Introduction

## Pangenome Graphs

- Model full genomic information of a species or clade
- Represent mutual relationships between all genomes

## Limitations of Current Methods

- Reference bias in traditional approaches
- Challenges with tree-based methods

## Introducing Implicit Pangenome Graphs

- Novel approach to overcome existing limitations
- Potential for unbiased, scalable comparative genomics

---

# 2. The Challenge in Comparative Genomics

## Existing Methods (e.g., Cactus)

- Based on phylogenetic trees
- Provides scalability for multiple genome alignment

## Limitations of Tree-Based Approaches

- Changing included genomes affects aligned regions
- Guide tree biases Incomplete Lineage Sorting (ILS) detection
- Updating alignments with new genomes is complex

## The Problem with Direct Graph Creation

- Computationally intensive ("graphs are STICKY")
- High memory requirements for loading entire graph
- Example: 35-day attempt with 16 T2T great ape genomes failed

## Need for a Scalable, Unbiased Approach

- Handle tens to thousands of genomes
- Avoid reference bias and input order dependency

---

# 3. Implicit Data Structures

## Definition and Concept

- Simulate full data structure without instantiating it
- Provide interface to query as if full structure exists

## Example: Implicit Interval Tree

- Used in bedtools and alignment algorithms
- Efficient for range overlap queries

## Benefits

- Memory efficiency
- Scalability to large datasets

## Application to Pangenome Graphs

- Avoiding materialization of full graph structure

---

# 4. The Implicit Pangenome Graph Concept

## Core Idea

- Graph implied by set of pairwise alignments
- Never materialize the full graph structure

## Key Components

1. Intervals as alignment ranges over target references
2. Coordinate translation as the fundamental operation

## Mathematical Foundation

- Transitive closure of matches
- Union-find operations for match closures

## Advantages over Materialized Graphs

- Reduced memory requirements
- Scalability to large numbers of genomes
- Flexibility in representation of genomic relationships

---

# 5. impg: Implementation of the Implicit Pangenome Graph

## Tool Architecture

- Input: bgzip-compressed PAF file of alignments
- Output: BED, BEDPE, PAF formats

## Key Data Structures

- Implicit interval tree for efficient range queries
- Compressed suffix array (CSA) for sequence indexing

## Core Operations

1. Stabbing query on the implicit interval tree
2. CIGAR string traversal for position-level translation

## Transitive Collection of Aligned Sequences

- Algorithm for collecting all mutually aligned sequences

## Optimizations

- Match compression for reduced memory usage
- Partitioning for parallel processing

## Comparison to seqwish Algorithm

- Similarities in graph induction process
- Differences in materialization and memory usage

---

# 6. Applications and Results

## T2T Primate Project Overview

- All-vs-all alignment of primate genomes
- Includes HPRC year 1 human genome assemblies

## Pangenome Alignment Process

1. Iterative all-vs-one approach using wfmash
2. Conversion to CHAIN format using paf2chain

## Subgraph Analysis

### MHC Locus

- Extraction process using impg
- Visualization and analysis of structural variants

### 8p23.1 Inversion

- Defensin gene content and polymorphism
- Graph construction and visualization

## SNP vs. Gap Divergence Analysis

- Computation method for 1MB segments
- Results for human-NHP comparisons
- Within-species autosome divergence

## Conservation Analyses

- PhastCons approach
- Distribution of conservation scores
- Gene conservation analysis
- Comparison with probability of loss intolerance (pLI)

---

# 7. Future Directions and Ongoing Work

## Developing a Total Subdivising Algorithm

- Generalization of the seqwish algorithm
- Potential for distributed construction of large comparative genomic pangenome graphs

## Potential Applications

- Analysis of structural variants across species
- Evolutionary studies and ancestral genome reconstruction

## Integration with Other Tools

- Combination with multiple sequence alignment tools
- Development of specialized visualization techniques

## Scaling to Larger Datasets

- Strategies for handling thousands of genomes
- Potential for cloud-based distributed processing

---

# 8. Conclusion

## Recap of Implicit Pangenome Graphs

- Key advantages: scalability, flexibility, unbiased representation

## Impact on Comparative Genomics

- Revolutionizing analysis of genomic relationships
- Enabling large-scale, dynamic genomic analyses

## Call for Collaboration

- Open-source project
- Opportunities for method refinement and application

---

# Thank You

Questions?