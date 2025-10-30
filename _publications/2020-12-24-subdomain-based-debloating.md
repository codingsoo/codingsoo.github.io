---
title: "Subdomain-Based Generality-Aware Debloating"
collection: publications
category: conferences
permalink: /publication/2020-subdomain-based-debloating
excerpt: 'This paper proposes DomGad, a debloating technique that produces reduced programs guaranteed to work for subdomains rather than specific inputs. Using stochastic optimization, it generates debloated programs achieving close-to-optimal tradeoff between reduction and generality, with results showing 50% code reduction and 95% generality on average.'
date: 2020-12-24
venue: '2020 35th IEEE/ACM International Conference on Automated Software Engineering (ASE)'
paperurl: 'https://doi.org/10.1145/3324884.3416644'
citation: 'Qi Xin, Myeongsoo Kim, Qirun Zhang, and Alessandro Orso. 2020. Subdomain-Based Generality-Aware Debloating. In 35th IEEE/ACM International Conference on Automated Software Engineering (ASE 20), September 21–25, 2020, Virtual Event, Australia. ACM, New York, NY, USA, 13 pages.'
---

Programs are becoming increasingly complex and typically contain an abundance of unneeded features, which can degrade the performance and security of the software. Recently, we have witnessed a surge of debloating techniques that aim to create a reduced version of a program by eliminating the unneeded features therein.

**Resources:**
- [ACM Digital Library](https://doi.org/10.1145/3324884.3416644)
- [Project Website](https://sites.google.com/view/domgad/)

## Problem with Existing Approaches

To debloat a program, most existing techniques require a usage profile of the program, typically provided as a set of inputs I. Unfortunately, these techniques tend to generate a reduced program that is **overfitted to I** and thus fails to behave correctly for other inputs outside of that specific set.

## Our Approach: DomGad

To address this limitation, we propose **DomGad** (Domain-based Generality-Aware Debloating), which has two main advantages over existing debloating approaches:

1. **Subdomain Guarantees**: It produces a reduced program that is guaranteed to work for **subdomains**, rather than for specific inputs
2. **Stochastic Optimization**: It uses stochastic optimization to generate reduced programs that achieve a close-to-optimal tradeoff between reduction and generality (i.e., the extent to which the reduced program is able to correctly handle inputs in its whole domain)

## Key Concepts

### Subdomains
Rather than ensuring correctness only for specific test inputs, DomGad aims to preserve program behavior for entire **subdomains** - structured regions of the input space that share common characteristics. This provides stronger guarantees about the debloated program's correctness.

### Generality
Generality measures the extent to which a reduced program correctly handles inputs across its intended domain. A program with high generality maintains correct behavior for a broader range of inputs, not just the specific test cases used during debloating.

## Evaluation

We assessed the effectiveness of DomGad by applying our approach to a benchmark of **ten Unix utility programs**.

### Results

Our results are promising, showing that DomGad could produce debloated programs that achieve, on average:
- **50% code reduction**
- **95% generality**

### Comparison with State-of-the-Art

Our results also show that DomGad performs well when compared with two state-of-the-art debloating approaches, demonstrating its effectiveness at balancing reduction and generality.

## Key Contributions

1. A novel subdomain-based approach to program debloating that provides stronger correctness guarantees
2. DomGad: an implementation using stochastic optimization for generality-aware debloating
3. Comprehensive evaluation on Unix utility programs demonstrating practical effectiveness
4. Demonstrated superior balance between code reduction and program generality compared to existing techniques

## BibTeX

```bibtex
@inproceedings{xin2020subdomain,
  title={Subdomain-based generality-aware debloating},
  author={Xin, Qi and Kim, Myeongsoo and Zhang, Qirun and Orso, Alessandro},
  booktitle={Proceedings of the 35th IEEE/ACM International Conference on Automated Software Engineering},
  pages={224--236},
  year={2020},
  organization={IEEE/ACM}
}
```
