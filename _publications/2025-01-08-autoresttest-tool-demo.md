---
title: "AutoRestTest: A Tool for Automated REST API Testing Using LLMs and MARL"
collection: publications
category: conferences
permalink: /publication/2025-autoresttest-tool-demo
excerpt: 'This paper presents a tool demonstration of AutoRestTest, an automated REST API testing tool that combines multi-agent reinforcement learning (MARL) with semantic property dependency graphs (SPDG) and Large Language Models (LLMs) for enhanced test generation and fault detection.'
date: 2025-01-08
venue: '2025 IEEE/ACM 47th International Conference on Software Engineering: Companion Proceedings (ICSE-Companion)'
paperurl: 'https://doi.org/10.1109/ICSE-Companion66252.2025.00015'
citation: 'T. Stennett, M. Kim, S. Sinha and A. Orso, "AutoRestTest: A Tool for Automated REST API Testing Using LLMs and MARL," in 2025 IEEE/ACM 47th International Conference on Software Engineering: Companion Proceedings (ICSE-Companion), Ottawa, ON, Canada, 2025, pp. 21-24, doi: 10.1109/ICSE-Companion66252.2025.00015.'
---

AutoRestTest is a novel REST API testing tool that integrates the Semantic Property Dependency Graph (SPDG) with Multi-Agent Reinforcement Learning (MARL) and Large Language Models (LLMs) for effective REST API testing. This tool demonstration paper presents AutoRestTest's architecture, features, and usage, along with preliminary evaluation results.

**Resources:**
- [IEEE Xplore](https://doi.org/10.1109/ICSE-Companion66252.2025.00015)
- [arXiv Paper](https://arxiv.org/abs/2501.08600)
- [GitHub Repository](https://github.com/selab-gatech/AutoRestTest)
- [Demo Video](https://www.youtube.com/watch?v=VVus2W8rap8)

## Tool Overview

AutoRestTest addresses key limitations in existing REST API testing tools by:

1. **Multi-Agent Collaboration:** Five specialized agents (Operation, Parameter, Value, Dependency, and Header agents) work together using MARL to optimize test generation
2. **Semantic Dependency Discovery:** SPDG reduces dependency search space using similarity-based modeling and runtime refinement
3. **Intelligent Value Generation:** LLMs generate contextually appropriate parameter values that satisfy domain-specific constraints

## Key Features

**Automated Test Generation:**
- Parses OpenAPI specifications
- Generates comprehensive test suites automatically
- Handles complex dependencies between API operations

**Multi-Agent Reinforcement Learning:**
- Coordinated optimization across five specialized agents (operation, parameter, value, dependency, and header)
- Value decomposition for collaborative learning
- Epsilon-greedy strategy with epsilon decay for exploration-exploitation balance

**Semantic Property Dependency Graph:**
- Efficient dependency modeling using semantic similarity
- Runtime discovery of undocumented dependencies
- Dynamic refinement based on testing feedback

**LLM-Driven Value Generation:**
- Context-aware parameter value generation
- Handles domain-specific constraints (emails, IDs, patterns)
- Few-shot prompting for optimal results

**Comprehensive Coverage:**
- Superior code coverage compared to state-of-the-art tools
- Enhanced fault detection capabilities
- Effective testing of complex online services

## Tool Demonstration

The tool demonstration includes:

1. **Setup and Configuration:** Installing AutoRestTest and preparing OpenAPI specifications
2. **Automated Testing:** Running AutoRestTest on real-world REST APIs
3. **Results Analysis:** Examining coverage reports and detected faults
4. **Comparison:** Side-by-side comparison with existing tools (RESTler, EvoMaster, MoRest, ARAT-RL)

## Preliminary Results

Evaluated on **4 real-world REST services** (FDIC, OMDb, OhSome, Spotify) comparing against four state-of-the-art tools (RESTler, EvoMaster, MoRest, ARAT-RL):

**Operations Successfully Exercised:**

| Service | AutoRestTest | ARAT-RL | EvoMaster | MoRest | RESTler |
|---------|--------------|---------|-----------|---------|---------|
| FDIC | 6 | 6 | 6 | 6 | 6 |
| OMDb | 1 | 1 | 1 | 1 | 1 |
| OhSome | **12** | 0 | 0 | 0 | 0 |
| Spotify | **7** | 5 | 4 | 4 | 3 |
| **Total** | **26** | 12 | 11 | 11 | 10 |

**Key Achievements:**
- Only tool to successfully process operations (2xx responses) on OhSome, one of the most challenging RESTful services
- Best performer on Spotify API with 7 operations exercised
- Only tool to detect a 5xx server error on the Spotify service (reported to developers)
- 117% more operations than RESTler
- 136% more operations than MoRest/EvoMaster
- 117% more operations than ARAT-RL

## BibTeX

```bibtex
@inproceedings{stennett2025autoresttest,
  title={AutoRestTest: A Tool for Automated REST API Testing Using LLMs and MARL},
  author={Stennett, Tyler and Kim, Myeongsoo and Sinha, Saurabh and Orso, Alessandro},
  booktitle={2025 IEEE/ACM 47th International Conference on Software Engineering: Companion Proceedings (ICSE-Companion)},
  pages={21--24},
  year={2025},
  organization={IEEE},
  doi={10.1109/ICSE-Companion66252.2025.00015}
}
```
