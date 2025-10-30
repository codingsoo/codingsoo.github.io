---
title: "Improving Program Debloating with 1-DU Chain Minimality"
collection: publications
category: conferences
permalink: /publication/2024-debloating-1du-chain-minimality
excerpt: 'This paper introduces RLDebloatDU, a novel debloating technique that employs 1-DU chain minimality within abstract syntax trees to maintain essential program data dependencies. The approach strikes a balance between aggressive code reduction and preservation of program semantics, significantly reducing CVEs while maintaining soundness better than both aggressive and conservative baseline approaches.'
date: 2024-05-23
venue: '2024 IEEE/ACM 46th International Conference on Software Engineering: Companion Proceedings (ICSE-Poster)'
paperurl: 'https://doi.org/10.1145/3639478.3643518'
citation: 'Myeongsoo Kim, Santosh Pande, and Alessandro Orso. 2024. Improving Program Debloating with 1-DU Chain Minimality. In Proceedings of the 2024 IEEE/ACM 46th International Conference on Software Engineering: Companion Proceedings (ICSE-Companion 24). Association for Computing Machinery, New York, NY, USA, 384–385. https://doi.org/10.1145/3639478.3643518'
---

Modern software often struggles with bloat, leading to increased memory consumption and security vulnerabilities from unused code. In response, various program debloating techniques have been developed, but they typically fall into two extremes: aggressive approaches that risk reintroducing vulnerabilities, or conservative strategies that retain too much code.

**Resources:**
- [ACM DL (Public Access)](https://dl.acm.org/doi/10.1145/3639478.3643518)
- [GitHub Repository](https://github.com/codingsoo/RLDebloatDU)

## The Debloating Dilemma

Software debloating techniques face a fundamental trade-off:

**Aggressive Approaches (e.g., Chisel):**
- ✅ Maximize code reduction
- ✅ Significant security improvements
- ❌ May overfit to test cases
- ❌ Risk reintroducing resolved vulnerabilities

**Conservative Approaches (e.g., Razor):**
- ✅ Preserve all influenced code
- ✅ Avoid reintroducing past security issues
- ❌ Less effective bloat reduction
- ❌ Retain unnecessary security attack surface

## Our Approach: RLDebloatDU

We introduce **RLDebloatDU**, an innovative debloating technique that employs **1-DU chain minimality** within abstract syntax trees to maintain essential program data dependencies, achieving a balance between aggressive code reduction and preservation of program semantics.

### What is 1-DU Chain Minimality?

**DU (Definition-Use) Chain:**
A DU chain represents the sequence of definitions and uses of a variable within a program, ensuring each definition reaches a use without being redefined.

**1-DU Chain Minimality:**
A program achieves 1-DU chain minimality when removing any single DU chain would break the program's desired functionality. This concept extends traditional 1-minimality to focus specifically on data dependencies.

**Advantages:**
- Preserves essential data semantics by maintaining all needed DU chains
- More precise than control-flow-based heuristics
- Balances code reduction with semantic preservation

## Architecture

RLDebloatDU integrates several key components systematically:

```
Source Code → AST Code Parser → PriorityQLearningAgent
                                        ↓
Test Script Runner ← DebloatManager ← Delta Debugging Partition Generator
```

### Component Details

**1. AST Code Parser:**
- Analyzes codebase using Abstract Syntax Trees
- Identifies debloatable segments based on DU chains
- Targets functions, global/local variables, and their Def-Use chains

**2. PriorityQLearningAgent:**
- Employs Q-Learning to determine optimal debloating policy
- Learns which code elements to prioritize for removal
- Adapts strategy based on test results

**3. DebloatManager:**
- Coordinates the debloating process
- Ensures functional integrity throughout
- Orchestrates iterative refinement

**4. Delta Debugging Partition Generator:**
- Efficiently generates code partitions for testing
- Enables systematic exploration of debloating space
- Accelerates the search process

**5. Test Script Runner:**
- Executes test cases on debloated code
- Verifies continued functionality
- Provides feedback to Q-Learning agent

### Iterative Process

RLDebloatDU operates in an iterative loop:
1. Analyze code and identify DU chains
2. Update Q-values based on test results
3. Generate progressively leaner code versions
4. Repeat until no further code size reduction is possible

## Evaluation

Evaluated on **10 Linux kernel programs** from Chisel's benchmark using Google Cloud E2 machines with Ubuntu 20.04.

### Security Improvements (CVE Reduction)

**Total CVEs:**
- Original code: **10 CVEs**
- Chisel (aggressive): **7 CVEs** (30% reduction)
- Razor (conservative): **6 CVEs** (40% reduction)
- **RLDebloatDU: 4 CVEs (60% reduction)** ✓

**Key Finding:** RLDebloatDU was as effective as Chisel in removing CVEs, but **without reintroducing previously resolved security vulnerabilities**.

### Gadget Reduction

Gadgets (ROP, JOP, COP, and special purpose) represent potential attack vectors:

| Metric | RLDebloatDU | Chisel | Razor |
|--------|-------------|---------|-------|
| **Total Gadgets** | **63.39%** ↓ | 65.26% ↓ | 36.7% ↓ |
| ROP Gadgets | 56.23% ↓ | 57.29% ↓ | 29.83% ↓ |
| JOP Gadgets | 75.82% ↓ | 77.72% ↓ | 61.68% ↓ |
| COP Gadgets | 72.58% ↓ | 77.72% ↓ | 30.52% ↓ |
| Special Purpose | 98.57% ↓ | 94.29% ↓ | 58.26% ↓ |

**Results:** RLDebloatDU achieved **63.39%** average gadget reduction, significantly better than Razor's 36.7% and closely comparable to Chisel's 65.26%.

### Binary Size Reduction

**Executable Segment Reduction:**
- RLDebloatDU: **83.57%** ↓
- Chisel: 89.71% ↓
- Razor: 54.37% ↓

RLDebloatDU achieved substantial size reduction (83.57%), slightly below Chisel but considerably higher than Razor.

### Soundness Evaluation

Testing with Razor's comprehensive test suite (299 tests for 10 Linux programs):

**Program Crashes:**
- **RLDebloatDU: 1 program crashed** (Tar) ✓
- Chisel: 4 programs crashed (Bzip2, Grep, Gzip, Tar)
- Razor: 4 programs crashed (Chown, Date, Rm, Tar)

**Conclusion:** RLDebloatDU demonstrates **superior soundness** compared to both Chisel and Razor.

## Detailed Results by Program

### Gadget Reduction Per Program

| Program | RLDebloatDU | Chisel | Razor |
|---------|-------------|---------|-------|
| Bzip2-1.0.5 | 34.2% | 42.4% | 3.8% |
| Chown-8.2 | 73% | 72.8% | 44% |
| Date-8.21 | 42.3% | 50.3% | 24.4% |
| Grep-2.19 | 73.4% | 69.7% | 59.5% |
| Gzip-1.2.4 | 57.5% | 63.3% | 62.4% |
| Mkdir-5.2.1 | 54.4% | 52.3% | 10.1% |
| Rm-8.4 | 70.7% | 69.4% | 25% |
| Sort-8.16 | 76.1% | 78.8% | 31.7% |
| Tar-1.14 | 82.8% | 84.9% | 69.9% |
| Uniq-8.16 | 69.5% | 68.7% | 36.2% |

## Key Advantages

**1. Balanced Approach:**
- Combines benefits of aggressive and conservative debloating
- Maximizes security improvements without sacrificing soundness

**2. Data-Dependency Focus:**
- More precise than control-flow-based heuristics
- Preserves semantic correctness through DU chain analysis

**3. Reinforcement Learning Integration:**
- Accelerates delta debugging process
- Enables efficient exploration of debloating search space
- Adapts strategy based on feedback

**4. Superior Security:**
- Highest CVE reduction (60%)
- No reintroduction of resolved vulnerabilities
- Comparable gadget reduction to aggressive approaches

**5. Better Soundness:**
- Fewer program crashes than both baselines
- More reliable debloated programs

## Benchmark Programs

Evaluated on 10 Linux kernel utilities:
- **Bzip2-1.0.5** - Compression utility
- **Chown-8.2** - File ownership utility
- **Date-8.21** - Date/time utility
- **Grep-2.19** - Pattern matching utility
- **Gzip-1.2.4** - Compression utility
- **Mkdir-5.2.1** - Directory creation utility
- **Rm-8.4** - File removal utility
- **Sort-8.16** - Text sorting utility
- **Tar-1.14** - Archive utility
- **Uniq-8.16** - Duplicate line removal utility

## Comparison with Related Techniques

### Chisel (Aggressive Approach)
**Strategy:** 1-tree minimality with reinforcement learning
**Strengths:** Maximum code reduction, effective CVE removal
**Weakness:** May reintroduce resolved vulnerabilities (4 program crashes)

### Razor (Conservative Approach)
**Strategy:** Control-flow-based heuristics
**Strengths:** Preserves influenced code, avoids reintroducing vulnerabilities
**Weakness:** Less effective debloating (only 54.37% size reduction, 4 program crashes)

### RLDebloatDU (Balanced Approach)
**Strategy:** 1-DU chain minimality with reinforcement learning
**Advantages:** Best of both worlds - aggressive security improvements with conservative soundness

## Key Contributions

1. **Novel 1-DU Chain Minimality Concept:** First application of DU chain analysis to debloating for precise semantic preservation
2. **RLDebloatDU Tool:** Practical implementation combining RL-guided delta debugging with DU chain analysis
3. **Balanced Debloating:** Demonstrates that aggressive security improvements don't require sacrificing soundness
4. **Comprehensive Evaluation:** Extensive comparison on 10 Linux programs with multiple metrics (CVEs, gadgets, size, soundness)
5. **Open-Source Artifact:** Publicly available tool for reproducibility and future research

## BibTeX

```bibtex
@inproceedings{kim2024debloating,
author = {Kim, Myeongsoo and Pande, Santosh and Orso, Alessandro},
title = {Improving Program Debloating with 1-DU Chain Minimality},
year = {2024},
isbn = {9798400705021},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3639478.3643518},
doi = {10.1145/3639478.3643518},
pages = {384–385},
numpages = {2},
location = {Lisbon, Portugal},
series = {ICSE-Companion '24}
}
```