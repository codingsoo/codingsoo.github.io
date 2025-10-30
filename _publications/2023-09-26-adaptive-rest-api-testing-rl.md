---
title: "Adaptive REST API Testing with Reinforcement Learning"
collection: publications
category: conferences
permalink: /publication/2024-adaptive-rest-api-testing-rl
excerpt: 'This paper presents ARAT-RL, an adaptive REST API testing technique that incorporates reinforcement learning to prioritize operations and parameters during exploration. The approach dynamically analyzes request and response data to inform dependent parameters and achieves superior code coverage and fault-detection capability compared to state-of-the-art tools.'
date: 2024-01-09
venue: 'Proceedings of the 38th IEEE/ACM International Conference on Automated Software Engineering (ASE 2023)'
paperurl: 'https://doi.org/10.1109/ASE56229.2023.00218'
citation: 'Myeongsoo Kim, Saurabh Sinha, and Alessandro Orso. 2024. Adaptive REST API Testing with Reinforcement Learning. In Proceedings of the 38th IEEE/ACM International Conference on Automated Software Engineering (ASE 2023). IEEE Press, 446–458.'
---

Modern web services increasingly rely on REST APIs. Effectively testing these APIs is challenging due to the vast search space to be explored, which involves selecting API operations for sequence creation, choosing parameters for each operation, and sampling values from the virtually infinite parameter input space.

**Resources:**
- [IEEE Xplore](https://doi.org/10.1109/ASE56229.2023.00218)
- [arXiv Paper](https://arxiv.org/pdf/2309.04583)
- [GitHub Repository & Artifact](https://github.com/codingsoo/ARAT-RL)

## Problem Statement

Current REST API testing tools face several limitations:

**Inefficient Exploration:**
- Treat all operations and parameters equally
- Lack prioritization strategies
- No consideration of operation/parameter importance or complexity

**Schema Dependency Issues:**
- Rely heavily on complete response schemas in specifications
- Struggle when schemas are absent or incomplete
- Cannot handle variant response formats

**Limited Adaptation:**
- Static testing strategies
- No learning from API feedback
- Inefficient handling of inter-parameter dependencies

## Our Approach: ARAT-RL

We present **ARAT-RL** (Adaptive REST API Testing with Reinforcement Learning), an advanced black-box testing approach with three innovative features:

### 1. Reinforcement Learning-based Prioritization

**Q-Learning for Operation Selection:**
- Assigns initial weights based on parameter usage frequency
- Continuously adjusts priorities based on API responses
- Negative rewards for successful operations (to explore other paths)
- Positive rewards for failing operations (to investigate further)

**Adaptive Parameter Selection:**
- Prioritizes parameters by Q-values
- Balances exploration vs. exploitation using ε-greedy strategy
- Dynamically adjusts based on testing feedback

**Value-Mapping Source Prioritization:**
Leverages five sources of parameter values:
1. Example values from specification
2. Random values based on type/format/constraints
3. Dynamic key-value pairs from requests
4. Dynamic key-value pairs from responses
5. Default values

### 2. Dynamic Key-Value Pair Construction

**Beyond Schema Analysis:**
- Analyzes POST operations to identify created resources
- Extracts key-value pairs from both requests and responses
- Works even when response schemas are incomplete or missing

**Hidden Dependency Discovery:**
- Uses Gestalt pattern matching to identify parameter relationships
- Discovers producer-consumer relationships not evident from specifications
- Handles plain-text responses and incomplete resource data

### 3. Sampling-based Strategy

**Efficient Response Processing:**
- Randomly samples key-value pairs from responses
- Reduces overhead of processing every response completely
- Maintains effectiveness while improving efficiency

## Key Features

**Intelligent Prioritization:**
- Initially weights operations by parameter usage frequency
- Adapts based on real-time API feedback
- Prevents redundant exploration of successful paths

**Flexible Value Generation:**
- Multiple value sources with learned preferences
- Handles regex patterns, constraints, and format specifications
- Extracts examples from natural language descriptions

**Dynamic Adaptation:**
- Learns from both successes and failures
- Adjusts exploration strategy during testing
- Discovers hidden dependencies through runtime analysis

## Evaluation Results

Evaluated on 10 RESTful services comparing against three state-of-the-art tools: RESTler, EvoMaster, and Morest.

### Code Coverage Achievements

**Average Coverage (ARAT-RL vs. best competitor):**
- **Branch coverage:** 36.25% (23.69% improvement over Morest)
- **Line coverage:** 58.47% (11.87% improvement over Morest)
- **Method coverage:** 59.42% (9.55% improvement over Morest)

**Comparison with All Tools:**
- **119%** more branch coverage than RESTler
- **60%** more line coverage than RESTler
- **52%** more method coverage than RESTler
- **37%** more branch coverage than EvoMaster
- **21%** more line coverage than EvoMaster
- **14%** more method coverage than EvoMaster

### Efficiency Metrics

**Requests Generated (1-hour budget):**
- **60,132** valid and fault-inducing requests on average
- **52%** more than Morest (39,595)
- **41%** more than EvoMaster (42,710)
- **1,222%** more than RESTler (4,550)

**Operations Covered:**
- **18 operations** on average
- **15%** more than Morest
- **24%** more than EvoMaster
- **283%** more than RESTler

### Fault Detection Capability

**Bugs Discovered:**
- **ARAT-RL:** 113 faults (average over 10 runs)
- **9.3x** more than RESTler (11 faults)
- **2.5x** more than EvoMaster (32 faults)
- **2.4x** more than Morest (33 faults)

**Key Insight:** ARAT-RL excelled particularly on services with larger parameter sets (e.g., LanguageTool with 11 parameters, Person Controller with 8 parameters), demonstrating the effectiveness of RL-based parameter combination exploration.

### Ablation Study Results

Impact of removing each component:

| Component Removed | Branch Coverage | Line Coverage | Method Coverage | Faults Detected |
|------------------|-----------------|---------------|-----------------|-----------------|
| Full ARAT-RL | 36.25% | 58.47% | 59.42% | 112.10 |
| No Prioritization | 28.70% (-26.3%) | 53.27% (-9.8%) | 55.51% (-7.0%) | 100.10 (-12%) |
| No Feedback | 32.69% (-10.9%) | 54.80% (-6.9%) | 56.09% (-5.9%) | 110.80 (-1.2%) |
| No Sampling | 34.10% (-6.3%) | 56.39% (-3.7%) | 57.20% (-3.9%) | 112.50 (-0.4%) |

**Key Finding:** Reinforcement learning-based prioritization contributes the most to effectiveness, followed by dynamic feedback analysis and sampling.

## Technical Approach

### Q-Learning Algorithm

**State Representation:**
- Operations and their parameters
- Current Q-values for operations and parameters
- Available value-mapping sources

**Action Space:**
- Select operation
- Choose parameters
- Pick value-mapping source

**Reward Function:**
- `-1` for successful responses (2xx) → explore other operations
- `+1` for failed responses (4xx, 5xx) → investigate further

**Update Rule:**
```
Q(s,a) ← Q(s,a) + α[r + γ max Q(s',a') - Q(s,a)]
```
where α = 0.1 (learning rate), γ = 0.99 (discount factor)

### Exploration Strategy

**ε-greedy Approach:**
- Initial ε = 1.0 (full exploration)
- Adaptive ε adjustment: ε ← min(ε_max, ε_adapt × ε)
- Balances exploitation of known good paths with exploration of new ones

## Benchmark Services

Evaluated on 10 real-world RESTful services:
- Features Service
- LanguageTool
- NCS
- REST Countries
- SCS
- Genome Nexus
- Person Controller
- User Management Microservice
- Market Service
- Project Tracking System

## Tools Compared

**RESTler:** Grammar-based fuzzing with stateful testing
**EvoMaster:** Evolutionary algorithm-based test generation (black-box mode)
**Morest:** Model-based testing with RESTful-service Property Graph

## Key Contributions

1. **Novel RL-based Prioritization:** First application of Q-learning to adaptively prioritize REST API operations and parameters during testing
2. **Dynamic Dependency Discovery:** Innovative approach to construct key-value pairs from requests and responses, handling incomplete schemas
3. **Sampling Strategy:** Efficient processing of API feedback through sampling-based key-value pair construction
4. **Comprehensive Evaluation:** Extensive empirical study demonstrating superior effectiveness, efficiency, and fault-detection capability
5. **Open-Source Artifact:** Publicly available tool, benchmark services, and experimental results

## Limitations and Future Work

**Current Limitations:**
- Struggles with semantic parameters requiring specific formats (e.g., email, address, phone number)
- Performance can be affected by services with strict formatting requirements

**Future Directions:**
- Combine with RESTful-service Property Graph (RPG) for better operation dependencies
- Multi-agent reinforcement learning approach
- Natural language analysis of server responses
- Large language model integration for operation sequencing
- Enhanced value generation using extensive parameter datasets
- Detection of wider variety of bugs beyond 500 status codes

## BibTeX

```bibtex
@inproceedings{10.1109/ASE56229.2023.00218,
  author = {Kim, Myeongsoo and Sinha, Saurabh and Orso, Alessandro},
  title = {Adaptive REST API Testing with Reinforcement Learning},
  year = {2024},
  isbn = {9798350329964},
  publisher = {IEEE Press},
  url = {https://doi.org/10.1109/ASE56229.2023.00218},
  doi = {10.1109/ASE56229.2023.00218},
  abstract = {Modern web services increasingly rely on REST APIs. Effectively testing these APIs is challenging due to the vast search space to be explored, which involves selecting API operations for sequence creation, choosing parameters for each operation from a potentially large set of parameters, and sampling values from the virtually infinite parameter input space. Current testing tools lack efficient exploration mechanisms, treating all operations and parameters equally (i.e., not considering their importance or complexity) and lacking prioritization strategies. Furthermore, these tools struggle when response schemas are absent in the specification or exhibit variants. To address these limitations, we present an adaptive REST API testing technique that incorporates reinforcement learning to prioritize operations and parameters during exploration. Our approach dynamically analyzes request and response data to inform dependent parameters and adopts a sampling-based strategy for efficient processing of dynamic API feedback. We evaluated our technique on ten RESTful services, comparing it against state-of-the-art REST testing tools with respect to code coverage achieved, requests generated, operations covered, and service failures triggered. Additionally, we performed an ablation study on prioritization, dynamic feedback analysis, and sampling to assess their individual effects. Our findings demonstrate that our approach outperforms existing REST API testing tools in terms of effectiveness, efficiency, and fault-finding ability.},
  booktitle = {Proceedings of the 38th IEEE/ACM International Conference on Automated Software Engineering},
  pages = {446–458},
  numpages = {13},
  keywords = {reinforcement learning for testing, automated rest API testing},
  location = {Echternach, Luxembourg},
  series = {ASE '23}
}
```