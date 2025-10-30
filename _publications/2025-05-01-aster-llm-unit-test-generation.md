---
title: "ASTER: Natural and Multi-Language Unit Test Generation with LLMs"
collection: publications
category: conferences
permalink: /publication/2025-aster-llm-unit-test-generation
excerpt: 'This paper presents ASTER, an LLM-assisted test generation technique guided by program analysis that generates natural and high-coverage unit tests for Java and Python. ASTER achieves competitive coverage with state-of-the-art tools while producing significantly more natural test cases that developers prefer. **Distinguished Paper Award** at ICSE-SEIP 2025.'
date: 2025-05-01
venue: '2025 IEEE/ACM 47th International Conference on Software Engineering: Software Engineering in Practice (ICSE-SEIP)'
paperurl: 'https://www.computer.org/csdl/proceedings-article/icse-seip/2025/368500a413/29kKWccGpK8'
citation: 'R. Pan, M. Kim, R. Krishna, R. Pavuluri and S. Sinha, "ASTER: Natural and Multi-Language Unit Test Generation with LLMs," in 2025 IEEE/ACM 47th International Conference on Software Engineering: Software Engineering in Practice (ICSE-SEIP), Ottawa, ON, Canada, 2025, pp. 413-424, doi: 10.1109/ICSE-SEIP66354.2025.00042.'
---

🏆 **Distinguished Paper Award** at ICSE-SEIP 2025

Implementing automated unit tests is an important but time-consuming activity in software development. While many automated test generation (ATG) techniques have been developed over the decades, they suffer from poor readability and do not resemble developer-written tests, inhibiting their adoption in practice. This paper presents **ASTER**, a rigorous investigation of how Large Language Models (LLMs) can bridge this gap.

**Resources:**
- [IEEE Computer Society Digital Library](https://www.computer.org/csdl/proceedings-article/icse-seip/2025/368500a413/29kKWccGpK8)
- [arXiv Preprint](https://arxiv.org/pdf/2409.03093)
- [GitHub Repository (Artifact)](https://github.com/aster-test-generation/aster)

## The Problem

Automated test generation tools have achieved considerable success in generating high-coverage test suites with good fault-detection ability, but they have several key limitations:

### Limitations of Existing ATG Tools

**Poor Test Naturalness:**
- ❌ Tests lack readability and comprehensibility
- ❌ Non-meaningful test and variable names (e.g., `test0()`, `stringArray1`)
- ❌ Trivial or ineffective assertions
- ❌ Contain anti-patterns and test smells
- ❌ Do not resemble developer-written tests

**Developer Perception Study Results:**
- **EvoSuite (Java):** 42% of developers would NOT add tests to regression suite, 21% only after significant changes
- **CodaMosa (Python):** 73% would not add tests, 14% only after significant changes
- **Consequence:** Developers find automatically generated tests hard to maintain and are reluctant to adopt them

**Limited Language Support:**
- Most tools work for very few programming languages
- Significant engineering effort required for each new language
- Complex applications requiring mocking often not supported

## Our Approach: ASTER

**ASTER** (Automated Software TEsting with LLMs for improved Readability) is a generic pipeline that incorporates static analysis to guide LLMs in generating compilable, high-coverage, and natural test cases.

### Key Innovation

Unlike existing approaches that rely on:
- Symbolic analysis (Pex, KLEE)
- Search-based techniques (EvoSuite, Pynguin)
- Random testing (Randoop)

ASTER leverages:
- **LLM's inherent ability** to synthesize natural-looking code
- **Static program analysis** to provide rich context
- **Multi-language support** through LLM's intrinsic knowledge
- **Mocking capability** for complex enterprise applications

### Multi-Language Support

ASTER is implemented for:
- **Java** (both Java SE and Java EE applications)
- **Python**

Demonstrating the feasibility of building multilingual unit test generators with LLMs guided by lightweight program analysis.

## Architecture and Workflow

```
Source Code → Preprocessing → LLM Prompting → Postprocessing → Enhanced Test Suite
               (Analysis)      (Generation)     (Repair)
```

### Three-Phase Pipeline

**1. Preprocessing Phase:**
Performs static analysis to extract context for LLM prompts:
- **Testing scope identification:** Public/protected methods, inherited implementations
- **Constructor chains:** Relevant constructors for creating required objects
- **Auxiliary methods:** Getters/setters for object state manipulation
- **Private method call chains:** Enabling indirect testing of private methods
- **Mocking candidates:** Fields, types, and methods requiring mocking

**2. LLM Prompting:**
Stage-specific prompts for:
- ① Initial test generation
- ② Test repair for compilation/runtime errors
- ③ Coverage augmentation

**3. Postprocessing Phase:**
- **Output sanitization:** Remove extraneous content, ensure syntax correctness
- **Error remediation:** Rule-based fixes + LLM-guided repair
- **Coverage augmentation:** Target uncovered code lines

### Mocking Support

ASTER incorporates comprehensive mocking capability for Java:

**Systematic Mocking Identification:**
1. **Fields and Types:** Identifies mockable APIs in focal class and method parameters
2. **Method Stubs:** Determines scope for "when-then" clauses

**Supported Mocking Patterns:**
- Field mocking
- Constructor mocking
- Static API mocking
- Non-static API mocking

**Example:** Apache Commons JXPath test with mocking:
```java
@Test
public void testisLeaf() throws Exception {
    node = mock(Node.class);
    domnodepointer = new DOMNodePointer(node, new Locale("en"), "id");
    when(node.hasChildNodes()).thenReturn(false);
    assertTrue(domnodepointer.isLeaf());
}
```

## Evaluation Setup

### Models Evaluated (6 LLMs)

| Model | Provider | Size | Type | License |
|-------|----------|------|------|---------|
| GPT-4-turbo | OpenAI | 1.76T† | Generic | Closed |
| Llama3-70b | Meta | 70B | Generic | Open |
| CodeLlama-34b | Meta | 34B | Code | Open |
| Granite-34b | IBM | 34B | Code | Open |
| Llama3-8b | Meta | 8B | Generic | Open |
| Granite-8b | IBM | 8B | Code | Open |

### Datasets

**Java SE Applications (4):**
- Apache Commons CLI (310 methods)
- Apache Commons Codec (977 methods)
- Apache Commons Compress (5,003 methods)
- Apache Commons JXPath (1,501 methods)

**Java EE Applications (4):**
- CargoTracker (1,074 methods)
- DayTrader (1,481 methods)
- PetClinic (238 methods)
- App X (proprietary enterprise app, 1,402 methods)

**Python Applications (283 modules):**
- 20 projects from CodaMosa benchmark
- 2,216 methods total

### Baselines

- **Java:** EvoSuite 1.2.0 (state-of-the-art evolutionary testing)
- **Python:** CodaMosa (latest LLM-enhanced search-based testing)

## Evaluation Results

### RQ1: Code Coverage Effectiveness

**Java SE Applications:**

ASTER is very competitive with EvoSuite:

| Application | Line Coverage | Branch Coverage | Method Coverage |
|-------------|---------------|-----------------|-----------------|
| Commons CLI | EvoSuite: 86% / ASTER: 82% | EvoSuite: 71% / ASTER: 74% | EvoSuite: 90% / ASTER: 88% |
| Commons Codec | EvoSuite: 84% / ASTER: 77% | EvoSuite: 76% / ASTER: 69% | EvoSuite: 89% / ASTER: 86% |
| Commons Compress | Similar low coverage (complex compression APIs) | | |
| **Commons JXPath** | EvoSuite: 13% / **ASTER: 66%** | EvoSuite: 11% / **ASTER: 51%** | EvoSuite: 16% / **ASTER: 67%** |

**Key Finding:** ASTER significantly outperforms on JXPath (4-5x better) due to mocking support.

**Java EE Applications:**

ASTER significantly outperforms EvoSuite:

| Application | Improvement |
|-------------|-------------|
| DayTrader | +26.4% line, +10.6% branch, +18.5% method |
| App X | **+7x line coverage, +84x branch coverage** |

**Python Applications:**

| Metric | CodaMosa | ASTER (GPT-4) | Improvement |
|--------|----------|---------------|-------------|
| Line Coverage | 44.0% | 78.0% | **+77%** |
| Branch Coverage | 53.7% | 77.2% | **+44%** |
| Method Coverage | 61.2% | 86.7% | **+42%** |

**Finding:** ASTER substantially outperforms CodaMosa across all metrics.

### Model Performance Comparison

**Surprising Result:** Smaller models (Granite-34b, Llama-8b) perform competitively with larger models:
- **Granite-34b:** Only -0.1% line coverage vs. GPT-4
- **Llama-8b:** Only -1.35% line coverage vs. GPT-4

**Implications:**
- Cost-effective alternative to GPT-4
- Suitable for enterprise deployment (on-premise, privacy-preserving)
- Reduced carbon footprint
- Can operate on workstations

### RQ2: Developer Perception

**Survey Methodology:**
- 161 professional software developers
- 9 focal methods with test pairs
- Questions on comprehensibility and usability

**Survey Results:**

**Java Tests:**
```
Question: "I understand what this test case is doing"
- ASTER: 91.6% positive (agree/strongly agree)
- EvoSuite: 67.7% positive
- Developer-written: 69.1% positive
```

**Python Tests:**
```
Question: "I understand what this test case is doing"
- ASTER: 100% positive
- CodaMosa: 45.1% positive
- Developer-written: 84.0% positive
```

**Readiness for Regression Suite:**

| Tool | Add without change | Minor changes | Significant changes | Will not add |
|------|-------------------|---------------|---------------------|--------------|
| ASTER (Java) | 30% | 40% | 22% | 8% |
| EvoSuite | 8% | 29% | 21% | 42% |
| ASTER (Python) | 48% | 40% | 10% | 2% |
| CodaMosa | 2% | 10% | 16% | 72% |

**Key Finding:** 70% (Java) and 88% (Python) of developers would add ASTER tests with no or minor changes.

### RQ3: Test Naturalness

**Automated Naturalness Metrics:**

**1. Test Name Meaningfulness:**
- ASTER: ~15% more natural than EvoSuite
- ASTER: ~23% more natural than developer-written tests
- Metric captures focal method inclusion and semantic relevance

**2. Variable Name Meaningfulness:**
```
EvoSuite Example:
  String[] stringArray1 = ...

ASTER Example:
  String[] flattenedArguments = ...
```

**3. Assertion Quality:**
- ASTER: 99% of Python tests have assertions (vs. 0% for CodaMosa baseline)
- ASTER: Fewer exception-related tests than EvoSuite (38% reduction)
- ASTER: More meaningful assertions checking expected transformations

**Example Comparison:**

**EvoSuite Generated:**
```java
@Test
public void test0() throws Throwable {
    BasicParser basicParser0 = new BasicParser();
    String[] stringArray0 = new String[4];
    String[] stringArray1 = basicParser0.flatten(
        (Options)null, stringArray0, true);
    assertEquals(4, stringArray1.length);
}
```

**ASTER Generated:**
```java
@Test
public void testFlattenWithShortOption_c5_o0() {
    Options options = new Options();
    options.addOption("option1", "o", true, "description1");
    String[] arguments = new String[] {
        "-o", "value1", "--option2", "value2"
    };
    BasicParser parser = new BasicParser();
    String[] flattenedArguments = parser.flatten(
        options, arguments, false);
    assertArrayEquals(new String[] {
        "-o", "value1", "-option2", "value2"
    }, flattenedArguments);
}
```

**Improvements:**
- ✅ Meaningful test name describing functionality
- ✅ Descriptive variable names (`flattenedArguments` vs. `stringArray1`)
- ✅ Assertion checks expected transformation, not just length

## Key Advantages of ASTER

**1. Superior Naturalness:**
- Meaningful test and variable names
- Quality assertions with semantic understanding
- Resembles developer-written tests
- High developer acceptance (70%+ Java, 88%+ Python)

**2. Competitive/Superior Coverage:**
- Java SE: Matches EvoSuite (-0.5% to +5.1%)
- Java EE: Significantly outperforms (+10.6% to +84x)
- Python: Substantially better than CodaMosa (+42% to +77%)

**3. Multi-Language Support:**
- Java (SE and EE)
- Python
- Extensible to other languages with lightweight analysis

**4. Mocking Capability:**
- Systematic identification of mocking candidates
- Supports complex enterprise applications
- Handles database operations, services, libraries

**5. Model Flexibility:**
- Works with 6 different LLMs
- Smaller models competitive with GPT-4
- Pluggable architecture

**6. Cost-Effective:**
- Smaller models (8B-34B) perform well
- On-premise deployment possible
- Privacy-preserving for enterprise use

## Industry Deployment

ASTER's Java test generation capability is offered in:
- **IBM watsonx Code Assistant for Enterprise Java Applications**

This demonstrates real-world applicability and industrial validation.

## Lessons Learned

### 1. Multi-Language/Framework Support

LLMs combined with lightweight program analysis offer versatile support for multiple languages and frameworks:
- Extends to new languages with minimal engineering effort
- Leverages LLM's intrinsic knowledge of syntax and semantics
- Critical for legacy and continuous development contexts

### 2. Naturalness is Achievable

LLMs trained on developer-written code inherently produce natural output:
- Superior to conventional ATG tools
- When combined with program analysis, achieves high coverage AND naturalness
- Research directions: enhance conventional tests, incorporate naturalness in pretraining

### 3. Affordability Through Smaller Models

Smaller models (8B-34B parameters) are competitive:
- Cost-effective alternatives to GPT-4
- Enable on-premise/local deployment
- Address privacy concerns in enterprise settings
- Future: develop efficient, quantized test-generation-specific models

## Related Work Comparison

**vs. Conventional ATG:**
- Symbolic analysis (KLEE, Pex)
- Search-based (EvoSuite, Pynguin)
- Random testing (Randoop)
- **ASTER:** Leverages LLMs for naturalness + multi-language support

**vs. Other LLM-based Testing:**
- AthenaTest: Fine-tuning pipeline
- TestPilot: JavaScript/TypeScript specific
- ChatUnitTest/ChatTester: GPT-specific, lacks program analysis
- Recent arXiv work: Builds on CodaMosa (lower baseline), single language
- **ASTER:** Multi-language, from-scratch generation, higher coverage, program analysis guidance

**vs. Industry Studies:**
- Meta study [Alshahwan et al. 2024]: 73% acceptance rate
- **ASTER:** 70%+ (Java), 88%+ (Python) acceptance
- Common finding: Naturalness key to acceptance
- **ASTER contribution:** Mocking support, multi-language, model size comparison, rigorous naturalness study

## Future Research Directions

### 1. Extended Language Support
- Apply to more programming languages (C++, Go, Rust)
- Different testing levels (integration, system testing)

### 2. Fine-Tuned Models
- Create test-generation-specific models
- Train on APIs-guru dataset (4,000+ specifications)
- Reduce cost of LLM interactions
- Lightweight models for commodity CPUs

### 3. Enhanced Fault Detection
- Beyond code coverage: fault-detection ability
- Semantic error detection
- Security vulnerability identification
- Mutation testing integration

### 4. RAG-Based Approaches
- Incorporate retrieval-augmented generation
- In-context learning with relevant examples
- Project-specific test pattern learning

### 5. Assertion Improvement
- Reduce tests without assertions
- Generate more meaningful assertions
- Property-based testing integration

## Key Contributions

1. **ASTER Technique:** Generic LLM-assisted test generation pipeline guided by static analysis
2. **Multi-Language Implementation:** Demonstrated for Java and Python
3. **Mocking Support:** Systematic approach for complex applications
4. **Comprehensive Evaluation:** 6 LLMs, 12 Java + 283 Python applications
5. **Superior Results:** Competitive coverage + significantly better naturalness
6. **Developer Study:** 161 professionals confirming developer preference
7. **Naturalness Analysis:** Automated metrics + qualitative assessment
8. **Industry Deployment:** IBM watsonx Code Assistant
9. **Open-Source Artifact:** Available for research community

## Implications

**For Practitioners:**
- Dramatically improved test quality and usability
- Tests ready for regression suites with minimal changes
- Support for complex enterprise applications
- Multi-language capability

**For Researchers:**
- Demonstrates LLM effectiveness for test generation
- Smaller models viable for resource-constrained settings
- Opens directions for naturalness-focused research
- Provides baseline and artifact for future work

**For Tool Developers:**
- Clear integration path for LLMs in testing tools
- Program analysis enhances LLM effectiveness
- Pluggable architecture supports different models
- Scalable solution across languages

## BibTeX

```bibtex
@INPROCEEDINGS{pan2025aster,
  author={Pan, Rangeet and Kim, Myeongsoo and Krishna, Rahul and Pavuluri, Raju and Sinha, Saurabh},
  booktitle={2025 IEEE/ACM 47th International Conference on Software Engineering: Software Engineering in Practice (ICSE-SEIP)}, 
  title={ASTER: Natural and Multi-Language Unit Test Generation with LLMs}, 
  year={2025},
  pages={413-424},
  doi={10.1109/ICSE-SEIP66354.2025.00042},
  address={Ottawa, ON, Canada},
  month={May},
  publisher={IEEE Computer Society},
  keywords={Java;Large language models;Pipelines;Static analysis;Software;Test pattern generators;Standards;Software engineering;Python;Software development management}
}
```