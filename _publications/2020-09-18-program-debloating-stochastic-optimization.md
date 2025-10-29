---
title: "Program debloating via stochastic optimization"
collection: publications
category: conferences
permalink: /publication/2020-program-debloating-stochastic-optimization
excerpt: 'This paper proposes a general approach that formulates program debloating as a multi-objective optimization problem. Debop, a specific instance of this approach, uses Markov Chain Monte Carlo (MCMC) sampling to consider three objectives: size reduction, attack-surface reduction, and generality to generate debloated programs that achieve optimal trade-offs between different debloating goals.'
date: 2020-09-18
venue: 'Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 20)'
paperurl: 'https://doi.org/10.1145/3377816.3381739'
citation: 'Qi Xin, Myeongsoo Kim, Qirun Zhang, and Alessandro Orso. 2020. Program debloating via stochastic optimization. In Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 20). Association for Computing Machinery, New York, NY, USA, 65–68.'
---

Programs typically provide a broad range of features. Because different typologies of users tend to use only a subset of these features, and unnecessary features can harm performance and security, program debloating techniques, which can reduce the size of a program by eliminating (possibly) unneeded features, are becoming increasingly popular.

## Problem Statement

Most existing debloating techniques tend to focus on program-size reduction alone and, although effective, ignore other important aspects of debloating. We believe that program debloating is a multifaceted problem that must be addressed in a more general way.

## Our Approach

In this spirit, we propose a **general approach** that formulates program debloating as a **multi-objective optimization problem**. Given a program to be debloated, our approach lets users specify:

1. A **usage profile** for the program (i.e., a set of inputs with associated usage probabilities)
2. The **factors of interest** for debloating
3. The **relative importance** of these factors (expressed as weights)

Based on this information, the approach defines a suitable objective function for associating a score to every possible reduced program and aims to generate an optimal solution that maximizes the objective function.

## Debop: A Multi-Objective Debloating Tool

We present and evaluate **Debop**, a specific instance of our approach that considers three objectives:

- **Size reduction (sr)**: Reducing the program's footprint measured in bytes
- **Attack-surface reduction (ar)**: Minimizing potential security vulnerabilities (measured by the number of ROP gadgets)
- **Generality (g)**: The extent to which the reduced program correctly handles inputs in the usage profile provided

### Technical Approach

Debop formulates the debloating problem as:

```
p_deb = arg max O(p, p')
        p'∈Sub(p)
```

where the objective function `O(p, p')` is defined as:
- General reduction: `r(p, p') = (1-α)·sr(p, p') + α·ar(p, p')`
- Overall objective: `O(p, p') = (1-β)·r(p, p') + β·g(p, p')`

The weights `α` and `β` (both between 0 and 1) control the relative importance of different objectives.

### Optimization via MCMC Sampling

Since enumerating all possible reduced programs is infeasible, Debop leverages **Markov Chain Monte Carlo (MCMC) sampling**, specifically the **Metropolis-Hastings algorithm**, to efficiently explore the search space and find approximate optimal solutions. The algorithm:

1. Starts with the original program
2. Iteratively generates new reduced programs by randomly adding/removing statements
3. Accepts or rejects candidates based on their O-score and acceptance probability
4. Avoids local maxima through probabilistic acceptance of worse solutions
5. Returns the sample with the highest O-score as the debloated program

## Implementation

Debop is implemented in C++ and uses:
- **Clang** for AST construction and statement identification
- **GCC** (v. 7.4.0, -O3) for compilation and size measurement
- **ROPgadget** tool for attack surface measurement (counting ROP gadgets)

## Evaluation

### Benchmark
We evaluated Debop on **mkdir** (version 5.2.1, ~28K LOCs), a Unix utility for creating directories, using:
- 26 tests provided with the program
- 2 additional tests from the BusyBox version
- 1,000 samples per trial
- 27 combinations of α (0.25, 0.5, 0.75) and β (0.1 to 0.9)

### Key Results

1. **Multiple Trade-offs (RQ1)**: Debop successfully generates debloated programs with different size reduction, attack-surface reduction, and generality trade-offs when provided with different α and β values. As β increases (favoring generality), generality scores increase while reduction scores decrease, confirming the approach's flexibility.

2. **Comparison with Chisel (RQ2)**: We compared Debop with **Chisel**, a state-of-the-art single-goal (size-only) debloating technique based on delta debugging:
   - Chisel achieves higher size reduction (Debop ratio: 0.82-0.86)
   - Debop achieves better attack-surface reduction (ratio: 1.07-1.13 vs. Chisel)
   - Debop's advantage: simultaneously optimizes multiple objectives, whereas Chisel focuses only on size

### Insights
- Debop pays a small price for generality compared to specialized techniques
- Multi-objective optimization provides better overall results when considering multiple goals
- The approach enables exploration of the Pareto frontier between conflicting objectives

## Key Contributions

1. A novel, general formulation of program debloating as a multi-objective optimization problem
2. Debop: an instance supporting size, attack-surface, and generality objectives via MCMC sampling
3. Proof-of-concept evaluation demonstrating effectiveness and trade-offs
4. Open-source implementation and experimental infrastructure at https://sites.google.com/view/debop19

## Publication Details

**Authors:** Qi Xin, Myeongsoo Kim, Qirun Zhang, Alessandro Orso

**Published in:** Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER '20)

**Conference:** May 23–29, 2020, Seoul, Republic of Korea

**Pages:** 65–68

**DOI:** [10.1145/3377816.3381739](https://doi.org/10.1145/3377816.3381739)

**Resources:**
- [ACM Digital Library](https://doi.org/10.1145/3377816.3381739)
- [Project Website](https://sites.google.com/view/debop19)

## BibTeX

```bibtex
@inproceedings{10.1145/3377816.3381739,
  author = {Xin, Qi and Kim, Myeongsoo and Zhang, Qirun Zhang and Orso, Alessandro},
  title = {Program debloating via stochastic optimization},
  year = {2020},
  isbn = {9781450371261},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3377816.3381739},
  doi = {10.1145/3377816.3381739},
  abstract = {Programs typically provide a broad range of features. Because different typologies of users tend to use only a subset of these features, and unnecessary features can harm performance and security, program debloating techniques, which can reduce the size of a program by eliminating (possibly) unneeded features, are becoming increasingly popular. Most existing debloating techniques tend to focus on program-size reduction alone and, although effective, ignore other important aspects of debloating. We believe that program debloating is a multifaceted problem that must be addressed in a more general way. In this spirit, we propose a general approach that allows for formulating program debloating as a multi-objective optimization problem. Given a program to be debloated, our approach lets users specify (1) a usage profile for the program (i.e., a set of inputs with associated usage probabilities), (2) the factors of interest for debloating, and (3) the relative importance of these factors. Based on this information, the approach defines a suitable objective function for associating a score to every possible reduced program and aims to generate an optimal solution that maximizes the objective function. We also present and evaluate Debop, a specific instance of our approach that considers three objectives: size reduction, attack-surface reduction, and generality (i.e., the extent to which the reduced program handles inputs in the usage profile provided). Our results, albeit still preliminary, are promising and show that our approach can be effective at generating debloated programs that achieve a good trade-off between the different de-bloating objectives considered. Our results also provide insights on the performance of our general approach when compared to a specialized single-goal technique.},
  booktitle = {Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results},
  pages = {65–68},
  numpages = {4},
  location = {Seoul, South Korea},
  series = {ICSE-NIER '20}
}
```