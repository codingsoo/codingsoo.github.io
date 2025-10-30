---
title: "Leveraging Large Language Models to Improve REST API Testing"
collection: publications
category: conferences
permalink: /publication/2024-restgpt-llm-rest-api-testing
excerpt: 'This paper presents RESTGPT, an innovative approach that leverages Large Language Models to improve REST API testing by extracting machine-interpretable rules and generating example parameter values from natural-language descriptions in API specifications. RESTGPT achieves 97% precision in rule extraction and 73% accuracy in value generation, significantly outperforming existing techniques.'
date: 2024-05-24
venue: 'Proceedings of the 2024 ACM/IEEE 44th International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 2024)'
paperurl: 'https://doi.org/10.1145/3639476.3639769'
citation: 'Myeongsoo Kim, Tyler Stennett, Dhruv Shah, Saurabh Sinha, and Alessandro Orso. 2024. Leveraging Large Language Models to Improve REST API Testing. In Proceedings of the 2024 ACM/IEEE 44th International Conference on Software Engineering: New Ideas and Emerging Results (ICSE-NIER 2024). Association for Computing Machinery, New York, NY, USA, 37–41.'
---

The widespread adoption of REST APIs, coupled with their growing complexity and size, has led to the need for automated REST API testing tools. However, current tools focus on structured data in REST API specifications but often neglect valuable insights available in unstructured natural-language descriptions, leading to suboptimal test coverage.

**Resources:**
- [ACM Digital Library (Open Access)](https://doi.org/10.1145/3639476.3639769)
- [GitHub Repository](https://github.com/selab-gatech/RESTGPT)

## The Problem

Automated REST API testing tools derive test cases primarily from API specifications like OpenAPI. However, they struggle to achieve high code coverage due to difficulties in comprehending the semantics and constraints present in parameter names and descriptions.

### Limitations of Existing Assistant Tools

**NLP2REST (NLP-based Approach):**
- ✅ Extracts constraints using keyword-driven extraction
- ❌ Limited precision (50% without validation, 79% with validation)
- ❌ Requires expensive validation process with deployed service
- ❌ Demands significant engineering effort
- ❌ Limited to specific rule types

**ARTE (Knowledge Base Approach):**
- ✅ Queries DBPedia for parameter values
- ❌ Low accuracy (16.93% on average)
- ❌ Context-blind: generates irrelevant values
- ❌ Cannot understand parameter context from descriptions
- ❌ Only 17% syntactically and semantically valid inputs

## Our Approach: RESTGPT

We introduce **RESTGPT**, an innovative approach that harnesses the power and intrinsic context-awareness of Large Language Models (LLMs) to improve REST API testing. RESTGPT extracts machine-interpretable rules and generates example parameter values from natural-language descriptions in API specifications.

### Key Innovation

Unlike existing approaches that rely on:
- Keyword matching (NLP2REST)
- External knowledge bases (ARTE)

RESTGPT leverages:
- **Semantic understanding** of LLMs
- **Context-awareness** from descriptions
- **Few-shot learning** for precise outputs
- **No validation required** - high precision without expensive checking

## Architecture and Workflow

```
OpenAPI Spec → Specification Parser → Rule Generator (GPT-3.5 Turbo)
                                              ↓
                                    Enhanced Specification
```

### Component Details

**1. Specification Parser:**
- Extracts machine-readable sections
- Identifies human-readable natural language descriptions
- Prepares data for rule extraction

**2. Rule Generator:**
Uses carefully crafted prompts to extract four constraint types:
1. **Operational Constraints:** Dependencies between operations
2. **Parameter Constraints:** Value ranges, formats, restrictions
3. **Parameter Type and Format:** Data types and structures
4. **Parameter Examples:** Sample valid values

**3. Specification Builder:**
- Combines extracted rules with original specification
- Ensures no conflicts between restrictions
- Produces enhanced OpenAPI specification

### Prompt Engineering Strategy

RESTGPT's effectiveness stems from its sophisticated prompt design with four core components:

**1. Guidelines:**
```
- Identify the parameter using its name and description
- Extract logical constraints from the parameter description
- Interpret the description in the least constraining way
```

**2. Cases (Chain-of-Thought):**
Decomposes rule extraction into specific, manageable pieces:
- Case 1: Non-definitive descriptions → Output "None"
- ...
- Case 10: Complex parameter relationships → Combine rules

**3. Grammar Highlights:**
Emphasizes key operators the model should recognize:
- Relational: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Arithmetic: `+`, `−`, `∗`, `/`
- Dependency: `AllOrNone`, `ZeroOrOne`, etc.

**4. Output Configurations:**
Defines structured output format for easy processing

### Few-Shot Learning

RESTGPT employs few-shot prompts with:
- Concise, contextually-rich instructions
- Representative examples
- Clear output formatting
- Ensures relevant and precise model outputs

## Motivating Example: FDIC Bank Data API

Consider this parameter from the FDIC Bank Data API specification:

```yaml
- name: filters
  description: |
    The filter for the bank search. Examples:
    * Filter by State name: STNAME:"West Virginia"
    * Filter for multiple States: STNAME:("West Virginia", "Delaware")

- name: sort_order
  description: |
    Indicator if ascending (ASC) or descending (DESC)
```

### Tool Comparison

**ARTE (Knowledge Base Approach):**
- `filters`: ❌ No relevant values (DBPedia has no context)
- `sort_order`: ❌ Generates "List of colonial heads of Portuguese Timor"

**NLP2REST (Keyword-Driven):**
- `filters`: ✓ Captures examples with "example" keyword
- `sort_order`: ❌ Fails without identifiable keywords

**RESTGPT (LLM-based):**
- `filters`: ✓ Generates `STNAME: "California"`, `STNAME: ("California", "New York")`
- `sort_order`: ✓ Correctly identifies "ASC" and "DESC"

**Result:** RESTGPT demonstrates superior contextual understanding.

## Evaluation Results

Evaluated on 9 RESTful services from the NLP2REST study with ground truth data.

### Rule Extraction Effectiveness

| Tool | Precision | Recall | F1 Score |
|------|-----------|--------|----------|
| NLP2REST (no validation) | 50% | 94% | 65% |
| NLP2REST (with validation) | 79% | 91% | 85% |
| **RESTGPT** | **97%** | **92%** | **94%** |

**Key Findings:**
- **97% precision** without any validation process
- Eliminates need for expensive validation infrastructure
- Outperforms NLP2REST even with validation (79%)
- Maintains high recall (92%) while achieving superior precision

### Detailed Results by Service

| Service | NLP2REST (no val) | NLP2REST (with val) | RESTGPT |
|---------|-------------------|---------------------|---------|
| FDIC | 54% / 93% | 93% / 93% | **100%** / 98% |
| Genome Nexus | 96% / 98% | 96% / 98% | **100%** / 93% |
| Language Tool | 63% / 100% | 90% / 90% | **100%** / 86% |
| OCVN | 88% / 88% | 93% / 76% | 88% / **94%** |
| OhSome | 16% / 93% | 52% / 80% | **80%** / 86% |
| OMDb | 100% / 100% | 100% / 100% | 100% / 100% |
| REST Countries | 97% / 88% | 100% / 88% | **100%** / 94% |
| Spotify | 55% / 94% | 75% / 93% | **98%** / 96% |
| YouTube | 19% / 88% | 76% / 82% | **92%** / 75% |

*Format: Precision / Recall*

### Value Generation Accuracy

Accuracy measures percentage of syntactically and semantically valid inputs generated:

| Service | ARTE | RESTGPT | Improvement |
|---------|------|---------|-------------|
| FDIC | 25.35% | **77.46%** | +206% |
| Genome Nexus | 9.21% | **38.16%** | +314% |
| Language Tool | 0% | **82.98%** | ∞ |
| OCVN | 33.73% | **39.76%** | +18% |
| OhSome | 4.88% | **87.80%** | +1,700% |
| OMDb | 36.00% | **96.00%** | +167% |
| REST Countries | 29.66% | **92.41%** | +212% |
| Spotify | 14.79% | **76.06%** | +414% |
| YouTube | 0% | **65.33%** | ∞ |
| **Average** | **16.93%** | **72.68%** | **+329%** |

**Key Achievement:** RESTGPT generates valid inputs for **73%** of parameters vs. **17%** for ARTE.

### Why RESTGPT Outperforms

**Context-Awareness Example (LanguageTool service):**
- **ARTE:** Generates generic language names: "Arabic", "Chinese", "English", "Spanish"
- **RESTGPT:** Understands context and generates proper language codes: "en-US", "de-DE"

This demonstrates LLM's superior ability to understand parameter context from descriptions.

## Key Advantages

**1. No Validation Required:**
- Achieves 97% precision without expensive validation
- No need for deployed service instance
- Reduced engineering effort

**2. Superior Context Understanding:**
- Interprets semantic meaning from descriptions
- Generates contextually relevant values
- Understands domain-specific conventions

**3. Broader Rule Coverage:**
- Not limited to specific keywords or patterns
- Handles complex constraint expressions
- Adapts to diverse specification styles

**4. High Accuracy:**
- 329% improvement over ARTE in value generation
- 94% improvement over NLP2REST without validation
- 23% improvement over NLP2REST with validation

**5. Scalability:**
- Works across diverse API domains
- No manual rule configuration required
- Generalizes to unseen specifications

## Benchmark Services

Evaluated on 9 real-world REST APIs:
- **FDIC** - Federal Deposit Insurance Corporation Bank Data
- **Genome Nexus** - Genomics data service
- **LanguageTool** - Grammar checking service
- **OCVN** - Open Contracting Vietnam
- **OhSome** - OpenStreetMap data analysis
- **OMDb** - Open Movie Database
- **REST Countries** - Country information service
- **Spotify** - Music streaming API
- **YouTube** - Video platform API

## Technical Implementation

**LLM Selection:** GPT-3.5 Turbo
- Chosen for accuracy and efficiency balance
- Demonstrated in OpenAI reports
- Cost-effective for practical deployment

**Prompt Components:**
1. **Guidelines:** Framework for rule interpretation
2. **Cases:** Chain-of-Thought decomposition
3. **Grammar:** Domain-specific operators
4. **Output Format:** Structured result templates

**Few-Shot Learning:**
- Provides contextually-rich examples
- Ensures precise, relevant outputs
- Reduces hallucination and errors

## Future Research Directions

### 1. Model Improvement

**Task-Specific Fine-Tuning:**
- Fine-tune LLMs using APIs-guru (4,000+ specifications)
- Incorporate RapidAPI dataset
- Enhance understanding of diverse API contexts

**Lightweight Model Development:**
- Create models for commodity CPUs
- Model pruning and compression
- Retain essential neurons for REST API tasks

### 2. Enhanced Fault Detection

**Beyond 500 Status Codes:**
- Detect CRUD semantic errors
- Identify producer-consumer relationship discrepancies
- Recognize logical inconsistencies

**Broader Bug Categories:**
- Security vulnerabilities
- Performance issues
- Specification-implementation mismatches

### 3. LLM-Based Testing Tool

**Server Message Leveraging:**
- Parse and understand server error messages
- Autonomously generate corrective test cases
- Adaptive testing based on feedback

**Dynamic Test Generation:**
- Real-time test adaptation
- Context-aware test sequences
- Intelligent exploration strategies

## Key Contributions

1. **RESTGPT Framework:** First application of LLMs to REST API specification enhancement
2. **Superior Performance:** 97% precision in rule extraction, 73% accuracy in value generation
3. **Validation-Free Approach:** High accuracy without expensive validation infrastructure
4. **Context-Aware Generation:** Semantic understanding enables contextually relevant outputs
5. **Open-Source Artifact:** Publicly available tool and experimental data
6. **Future Roadmap:** Clear directions for advancing LLM-based REST API testing

## Implications

**For Practitioners:**
- Dramatically improved test input quality
- Reduced manual effort in test creation
- Better coverage without additional engineering

**For Researchers:**
- Demonstrates LLM effectiveness for specification analysis
- Opens new directions for AI-assisted testing
- Provides baseline for future comparisons

**For Tool Developers:**
- Clear integration path for LLMs in testing tools
- Validation-free approach reduces complexity
- Scalable solution for diverse APIs

## BibTeX

```bibtex
@inproceedings{kim2024restapi,
  author = {Kim, Myeongsoo and Stennett, Tyler and Shah, Dhruv and Sinha, Saurabh and Orso, Alessandro},
  title = {Leveraging Large Language Models to Improve REST API Testing},
  year = {2024},
  isbn = {9798400705007},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  url = {https://doi.org/10.1145/3639476.3639769},
  doi = {10.1145/3639476.3639769},
  booktitle = {Proceedings of the 2024 ACM/IEEE 44th International Conference on Software Engineering: New Ideas and Emerging Results},
  pages = {37–41},
  numpages = {5},
  keywords = {large language models for testing, OpenAPI specification analysis},
  location = {Lisbon, Portugal},
  series = {ICSE-NIER'24}
}
```