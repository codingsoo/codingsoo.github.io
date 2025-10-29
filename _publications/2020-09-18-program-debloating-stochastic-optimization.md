---
title: "Program debloating via stochastic optimization"
collection: publications
category: conferences
permalink: /publication/2020-program-debloating-stochastic-optimization
excerpt: 'This paper proposes a general approach that formulates program debloating as a multi-objective optimization problem. Debop, a specific instance of this approach, considers three objectives: size reduction, attack-surface reduction, and generality to generate debloated programs that achieve a good trade-off between different debloating goals.'
date: 2020-09-18
venue: 'Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 20)'
paperurl: 'https://doi.org/10.1145/3377816.3381739'
citation: 'Qi Xin, Myeongsoo Kim, Qirun Zhang, and Alessandro Orso. 2020. Program debloating via stochastic optimization. In Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 20). Association for Computing Machinery, New York, NY, USA, 65–68.'
---

Programs typically provide a broad range of features. Because different typologies of users tend to use only a subset of these features, and unnecessary features can harm performance and security, program debloating techniques, which can reduce the size of a program by eliminating (possibly) unneeded features, are becoming increasingly popular.

## Abstract

Most existing debloating techniques tend to focus on program-size reduction alone and, although effective, ignore other important aspects of debloating. We believe that program debloating is a multifaceted problem that must be addressed in a more general way.

In this spirit, we propose a general approach that allows for formulating program debloating as a multi-objective optimization problem. Given a program to be debloated, our approach lets users specify:
1. A **usage profile** for the program (i.e., a set of inputs with associated usage probabilities)
2. The **factors of interest** for debloating
3. The **relative importance** of these factors

Based on this information, the approach defines a suitable objective function for associating a score to every possible reduced program and aims to generate an optimal solution that maximizes the objective function.

## Debop: A Multi-Objective Debloating Tool

We present and evaluate **Debop**, a specific instance of our approach that considers three objectives:
- **Size reduction** - reducing the program's footprint
- **Attack-surface reduction** - minimizing potential security vulnerabilities
- **Generality** - the extent to which the reduced program handles inputs in the usage profile provided

## Results

Our results, albeit still preliminary, are promising and show that our approach can be effective at generating debloated programs that achieve a good trade-off between the different debloating objectives considered. Our results also provide insights on the performance of our general approach when compared to a specialized single-goal technique.

**Authors:** Qi Xin, Myeongsoo Kim, Qirun Zhang, Alessandro Orso

**Published in:** Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER '20)

**Conference Location:** Seoul, South Korea

**Pages:** 65–68

**DOI:** [10.1145/3377816.3381739](https://doi.org/10.1145/3377816.3381739)

**Resources:**
- [ACM Digital Library](https://doi.org/10.1145/3377816.3381739)

## BibTeX

```bibtex
@inproceedings{10.1145/3377816.3381739,
  author = {Xin, Qi and Kim, Myeongsoo and Zhang, Qirun and Orso, Alessandro},
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
