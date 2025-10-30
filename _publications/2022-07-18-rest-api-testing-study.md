---
title: "Automated test generation for REST APIs: no time to rest yet"
collection: publications
category: conferences
permalink: /publication/2022-rest-api-testing-study
excerpt: 'This paper presents a comprehensive empirical study of 10 state-of-the-art REST API testing tools, evaluating their performance on 20 benchmarks in terms of code coverage and fault-detection ability. The study identifies key strengths, weaknesses, and limitations of existing approaches and provides concrete directions for future research.'
date: 2022-07-18
venue: 'Proceedings of the 31st ACM SIGSOFT International Symposium on Software Testing and Analysis (ISSTA 2022)'
paperurl: 'https://doi.org/10.1145/3533767.3534401'
citation: 'Myeongsoo Kim, Qi Xin, Saurabh Sinha, and Alessandro Orso. 2022. Automated test generation for REST APIs: no time to rest yet. In Proceedings of the 31st ACM SIGSOFT International Symposium on Software Testing and Analysis (ISSTA 2022). Association for Computing Machinery, New York, NY, USA, 289–301.'
---

Modern web services routinely provide REST APIs for clients to access their functionality. These APIs present unique challenges and opportunities for automated testing, driving the recent development of many techniques and tools that generate test cases for API endpoints using various strategies.

**Resources:**
- [ACM Digital Library (Open Access)](https://doi.org/10.1145/3533767.3534401)
- [Project Website](https://github.com/codingsoo/REST_Go)

## Problem Statement

Understanding how REST API testing techniques compare to one another is difficult, as they have been evaluated on different benchmarks and using different metrics. This creates a gap in understanding the landscape of automated REST API testing and makes it challenging to guide future research in this area.

## Our Approach

We performed a comprehensive empirical study to understand the landscape in automated testing of REST APIs. Our study involved:

1. **Systematic Tool Selection**: Through a thorough literature search, we identified 10 state-of-the-art REST API testing tools, including both academic and practitioners' tools
2. **Comprehensive Benchmark**: We applied these tools to 20 real-world open-source RESTful services
3. **Multi-faceted Evaluation**: We analyzed performance in terms of:
   - Code coverage achieved (lines, branches, methods)
   - Unique failures triggered (500 errors, failure points, library failure points)

## Tools Evaluated

The study included 10 tools with diverse testing strategies:

### White-box Tool
- **EvoMasterWB**: Uses evolutionary algorithms with code coverage feedback

### Black-box Tools
- **EvoMasterBB**: Random testing with stateful request generation
- **RESTler**: Dependency-based algorithm with dictionary-based fuzzing
- **RestTestGen**: Dependency-based with mutation and dynamic value generation
- **RESTest**: Model-based testing with constraint solving
- **Schemathesis**: Property-based testing
- **Dredd**: Sample-value-based testing
- **Tcases**: Model-based combinatorial testing
- **bBOXRT**: Robustness testing
- **APIFuzzer**: Random-mutation-based fuzzing

## Key Findings

### Coverage Results
- Overall, all tools achieved relatively low coverage on many benchmarks
- Best performer (EvoMasterWB) achieved less than 53% line coverage on average
- Best black-box tool (EvoMasterBB) achieved 45.41% line coverage

### Key Limitations Identified

1. **Parameter Value Generation**: Tools generate many invalid requests that are rejected by services due to:
   - Domain-specific value requirements
   - Data format restrictions
   - Lack of sophisticated input generation strategies

2. **Operation Dependency Detection**: Most tools either:
   - Do not account for producer-consumer dependencies between operations
   - Use simple heuristics leading to false positives/negatives
   - Fail to generate stateful tests effectively

3. **Specification-Implementation Mismatch**: Discrepancies between API specifications and actual implementations hinder tool effectiveness

### Fault Detection
- Strong positive correlation between code coverage and number of faults exposed
- Failures in library methods often have more serious consequences than failures in service code
- Exercising operations with different parameter combinations and input types helps reveal more faults

## Implications for Future Research

Based on our findings, we identify several promising directions:

1. **Better Input Parameter Generation**:
   - Leverage information embedded in API specifications
   - Extract sample values from parameter descriptions using NLP
   - Use symbolic execution for white-box approaches
   - Apply more sophisticated testing techniques (e.g., higher-level combinatorial testing)

2. **Improved Stateful Testing**:
   - Develop more accurate dependency detection mechanisms
   - Use static analysis when source code is available
   - Apply NLP techniques to specification descriptions
   - Leverage machine learning for dependency inference

3. **Enhanced Validation**:
   - Analyze server logs and error messages for guidance
   - Better handling of dynamic responses
   - Improved matching of fields across different types

## Proof-of-Concept Results

We implemented preliminary prototypes to validate our suggestions:

- **Parameter Description Analysis**: Identified developer-suggested values for 32% of parameters in two services
- **NLP-based Dependency Detection**: Automatically detected 8 of 12 unique inter-parameter dependencies using dependency parsing
- **Textual Similarity for Dependencies**: Correctly identified almost 80% of operations involved in dependency relationships using top-3 textual matches

## Key Contributions

1. A comprehensive empirical study of 10 REST API testing tools on 20 benchmarks
2. Analysis of strengths and weaknesses of existing techniques and their underlying strategies
3. Concrete suggestions for improvement with proof-of-concept evaluations
4. Implications for future research in REST API testing
5. Publicly available artifact with tools, benchmarks, and experimental infrastructure

## BibTeX

```bibtex
@inproceedings{10.1145/3533767.3534401,
  author = {Kim, Myeongsoo and Xin, Qi and Sinha, Saurabh and Orso, Alessandro},
  title = {Automated test generation for REST APIs: no time to rest yet},
  year = {2022},
  isbn = {9781450393799},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3533767.3534401},
  doi = {10.1145/3533767.3534401},
  booktitle = {Proceedings of the 31st ACM SIGSOFT International Symposium on Software Testing and Analysis},
  pages = {289–301},
  numpages = {13},
  keywords = {RESTful APIs, Automated software testing},
  location = {Virtual, South Korea},
  series = {ISSTA 2022}
}
```
