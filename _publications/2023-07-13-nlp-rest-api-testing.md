---
title: "Enhancing REST API Testing with NLP Techniques"
collection: publications
category: conferences
permalink: /publication/2023-nlp-rest-api-testing
excerpt: 'This paper presents NLPtoREST, an automated approach that applies natural language processing techniques to extract rules from the human-readable part of OpenAPI specifications. The technique generates enhanced specifications that significantly improve the performance of REST API testing tools, increasing coverage by up to 103% and successful request rates by 20%.'
date: 2023-07-13
venue: 'Proceedings of the 32nd ACM SIGSOFT International Symposium on Software Testing and Analysis (ISSTA 2023)'
paperurl: 'https://doi.org/10.1145/3597926.3598131'
citation: 'Myeongsoo Kim, Davide Corradini, Saurabh Sinha, Alessandro Orso, Michele Pasqua, Rachel Tzoref-Brill, and Mariano Ceccato. 2023. Enhancing REST API Testing with NLP Techniques. In Proceedings of the 32nd ACM SIGSOFT International Symposium on Software Testing and Analysis (ISSTA 2023). Association for Computing Machinery, New York, NY, USA, 1232–1243.'
---

RESTful services are commonly documented using OpenAPI specifications. Although numerous automated testing techniques have been proposed that leverage the machine-readable part of these specifications to guide test generation, their human-readable part has been mostly neglected.

**Resources:**
- [ACM Digital Library (Open Access)](https://doi.org/10.1145/3597926.3598131)
- [GitHub Repository](https://github.com/codingsoo/nlp2rest)

## Problem Statement

Natural-language descriptions in OpenAPI specifications often contain relevant information, including:
- Example values for parameters
- Inter-parameter dependencies
- Parameter constraints
- Format specifications

This information can significantly improve test generation, but existing REST API testing tools ignore the human-readable descriptions and only use the machine-readable portions of specifications.

## Our Approach: NLPtoREST

We propose **NLPtoREST**, an automated approach that applies natural language processing techniques to assist REST API testing. The approach consists of three main phases:

### 1. NLP-based Rule Extraction

**Vocabulary Terms Identification:**
- Uses a custom Word2Vec model (restW2V) trained on 1,875,607 text sets from 4,064 REST API specifications
- Identifies sentences containing OpenAPI vocabulary terms using semantic similarity
- Matches 55 mappings between search terms and OpenAPI keywords

**Value and Parameter Name Detection:**
- Uses regular expressions for enumerated or quoted strings
- Applies constituency parse tree analysis as fallback
- Extracts parameter names and values from natural language descriptions

**Rule Generation:**
- Creates OpenAPI-compliant rules from extracted information
- Supports four rule categories:
  - Parameter type/format rules
  - Parameter constraint rules
  - Parameter example rules
  - Operation constraint rules (inter-parameter dependencies)

### 2. Rule Validation

**Static Pruning:**
- Analyzes rule combinations for syntactic compatibility
- Discards combinations incompatible with OpenAPI standard
- Dramatically reduces combinations requiring dynamic checking

**Dynamic Checking:**
- Generates validation test cases for rule combinations
- Executes tests against deployed API instance
- Identifies maximal combination of valid rules

**Fine Tuning:**
- Further validates potentially valid rules through mutations
- Repairs inter-parameter dependency rules (Or, OnlyOne, AllOrNone, ZeroOrOne)
- Validates 22 out of 26 supported rule types with precise strategies

### 3. Enhanced Specification Generation

- Adds validated rules to original OpenAPI specification
- Uses OpenAPI-supported keywords and extensions
- Creates enhanced specification transparently usable by any REST API test generator

## Key Features

**Custom NLP Model:**
- Pre-trained Word2Vec model (restW2V) specific to OpenAPI terminology
- Trained on extensive dataset from thousands of API specifications
- Provides flexible matching beyond hard-coded patterns

**Comprehensive Rule Support:**
- Extracts 4 categories and 26 types of rules
- Handles complex inter-parameter dependencies
- Supports various constraint and format specifications

**Validation Pipeline:**
- Static analysis eliminates ~75% of false positives
- Dynamic validation against actual service implementation
- Fine-tuning phase repairs and validates remaining rules

## Evaluation Results

### Rule Extraction Effectiveness
- **Recall:** 94% (313 out of 333 rules extracted)
- **Precision before validation:** 50% (313 TP, 314 FP)
- **Precision after validation:** 79% (304 TP, 79 FP)
- **Overall improvement:** 58% increase in precision with only 3% decrease in recall

### Comparison with RestCT
NLPtoREST significantly outperformed RestCT, a pattern-matching-based approach:
- **NLPtoREST:** Identified 15 out of 19 inter-parameter dependencies
- **RestCT:** Identified 0 out of 19 inter-parameter dependencies

### Impact on Testing Tools
Enhanced specifications significantly improved performance of 8 state-of-the-art REST API testing tools:

**Coverage Improvements:**
- **Branch coverage:** +103% (11.35% → 23.10%)
- **Line coverage:** +50% (24.96% → 37.52%)
- **Method coverage:** +52% (22.13% → 33.54%)

**Request Success Rates:**
- **Successful requests (2XX):** +20% (21.8% → 26.2%)
- **Rejected requests (4XX):** -7% (59.9% → 56.0%)
- **Server errors (5XX):** +2% (18.3% → 18.6%)
- **Unique server errors:** +4% on average (up to +98.9% maximum)

## Tools Evaluated

The enhanced specifications improved performance across 8 testing tools:
- **EvoMasterBB:** Evolutionary algorithm-based testing
- **bBOXRT:** Robustness testing
- **Morest:** Model-based testing
- **RESTest:** Constraint-based testing
- **RESTler:** Stateful fuzzing
- **RestTestGen:** Dependency-based testing
- **Schemathesis:** Property-based testing
- **Tcases:** Combinatorial testing

## Key Contributions

1. A novel NLP-based technique for extracting rules from natural-language descriptions in OpenAPI specifications
2. A validation approach that significantly improves rule accuracy through static and dynamic checking
3. Enhanced OpenAPI specifications that existing REST API testing tools can transparently use
4. Comprehensive empirical evaluation on 9 real-world services demonstrating significant performance improvements
5. Publicly available tool and experimental infrastructure

## Benchmark Services

Evaluated on 9 industrial-sized REST services (>10K LoC):
- Federal Deposit Insurance Corporation (FDIC)
- LanguageTool
- OhSome
- Open Movie Database (OMDb)
- REST Countries
- Genome Nexus
- OCVN
- Spotify
- YouTube Mock

## BibTeX

```bibtex
@inproceedings{kim2023nlp,
  author = {Kim, Myeongsoo and Corradini, Davide and Sinha, Saurabh and Orso, Alessandro and Pasqua, Michele and Tzoref-Brill, Rachel and Ceccato, Mariano},
  title = {Enhancing REST API Testing with NLP Techniques},
  year = {2023},
  isbn = {9798400702211},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3597926.3598131},
  doi = {10.1145/3597926.3598131},
  booktitle = {Proceedings of the 32nd ACM SIGSOFT International Symposium on Software Testing and Analysis},
  pages = {1232–1243},
  numpages = {12},
  keywords = {OpenAPI Specification Analysis, Natural Language Processing for Testing, Automated REST API Testing},
  location = {Seattle, WA, USA},
  series = {ISSTA 2023}
}
```