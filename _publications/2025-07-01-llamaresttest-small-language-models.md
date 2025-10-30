---
title: "LlamaRestTest: Effective REST API Testing with Small Language Models"
collection: publications
category: manuscripts
permalink: /publication/2025-llamaresttest-small-language-models
excerpt: 'This paper presents LlamaRestTest, a novel REST API testing approach that employs two custom fine-tuned small language models (Llama3-8B) to generate realistic test inputs and uncover inter-parameter dependencies during testing by analyzing server responses. The approach demonstrates that fine-tuning enables smaller models to outperform much larger models in REST API testing, balancing effectiveness and efficiency.'
date: 2025-07-01
venue: 'Proceedings of the ACM on Software Engineering, Volume 2, FSE'
paperurl: 'https://doi.org/10.1145/3715737'
citation: 'Myeongsoo Kim, Saurabh Sinha, and Alessandro Orso. 2025. LlamaRestTest: Effective REST API Testing with Small Language Models. Proc. ACM Softw. Eng. 2, FSE, Article FSE022 (July 2025), 24 pages. https://doi.org/10.1145/3715737'
---

Modern web services rely heavily on REST APIs, typically documented using the OpenAPI specification. Although Large Language Models (LLMs) have shown promising test-generation abilities, their application to REST API testing remains mostly unexplored. This paper presents LlamaRestTest, a novel approach that demonstrates how small language models can perform as well as, or better than, large language models in REST API testing.

**Resources:**
- [ACM Digital Library (Open Access)](https://dl.acm.org/doi/10.1145/3715737)
- [arXiv Paper](https://arxiv.org/abs/2501.08598)
- [GitHub Repository](https://github.com/codingsoo/LlamaRestTest)

## Key Innovation

LlamaRestTest employs two custom LLMs created by fine-tuning and quantizing the Llama3-8B model:

1. **LlamaREST-IPD:** Identifies inter-parameter dependencies by analyzing parameter descriptions and server responses
2. **LlamaREST-EX:** Generates semantically valid test inputs that conform to API domain requirements

These models were fine-tuned using a comprehensive dataset combining:
- Martin-Lopez's dependency database (551 parameters with IPDs)
- Over 1.8 million API operation parameters mined from APIs-guru

## Approach

**Fine-Tuning Strategy:**
- Used Parameter-Efficient Fine-Tuning (PEFT) with QLoRA technique
- Trained on custom tokenized formats with special tokens for IPD rules and example values
- Applied early stopping to prevent overfitting (35 epochs for LlamaREST-EX, 51 for LlamaREST-IPD)

**Quantization Exploration:**
- Evaluated 2-bit, 4-bit, and 8-bit quantization levels
- Balanced model size reduction with accuracy maintenance
- 8-bit quantization achieved optimal effectiveness-efficiency tradeoff

**Integration with ARAT-RL:**
- Leverages reinforcement learning for adaptive API exploration
- Uses ε-greedy strategy to balance exploration and exploitation
- Activates LLMs upon repeated failures to refine parameter selection

**Dynamic Server Feedback:**
- Analyzes server response messages in real-time
- Refines test inputs based on error messages and API behavior
- Discovers hidden dependencies not documented in specifications

## Evaluation Results

Evaluated on **12 real-world services** (including Spotify, Language Tool, Ohsome) comparing against RESTGPT, RESTler, EvoMaster, MoRest, and ARAT-RL:

### Value Generation Performance

**Semantically Valid Input Generation:**
- LlamaREST-EX (fine-tuned): **72.44%** precision
- RESTGPT: 68.82% precision
- Llama3-8B (base model): 22.94% precision

LlamaREST-EX outperformed RESTGPT on challenging APIs:
- Ohsome: 92.16% vs 76.83%
- Genome-Nexus: 57.02% vs 36.25%

### Inter-Parameter Dependency Detection

**IPD Rules Correctly Identified:**
- LlamaREST-IPD (fine-tuned): **12 out of 17** rules
- RESTGPT: 9 rules
- Llama3-8B (base model): 2 rules

### Code Coverage (Open-Source Services)

**Average Coverage with 8-bit Quantization:**
- Method Coverage: **55.8%** (vs 41.7-45.8% for other tools)
  - +14.1 to +22.7 percentage points improvement
- Branch Coverage: **28.3%** (vs 9.6-17.8% for other tools)
  - +9.2 to +18.7 percentage points improvement
- Line Coverage: **55.3%** (vs 33.2-45.3% for other tools)
  - +10.0 to +22.1 percentage points improvement

### Operation Coverage (Online Services)

**Operations Successfully Processed:**
- LlamaRestTest: **37.5 operations**
- ARAT-RL: 11.6 operations
- EvoMaster: 11.2 operations
- MoRest: 9.9 operations
- RESTler: 10.6 operations

Notable achievements:
- Only tool to process operations on Ohsome (24.5 operations vs 0 for others)
- Best performance on Spotify (7 operations vs 3.9-5.6 for others)

### Fault Detection

**Internal Server Errors (500) Detected:**
- LlamaRestTest: **204 errors**
- ARAT-RL: 160 errors (+44 for LlamaRestTest)
- MoRest: 140 errors (+64 for LlamaRestTest)
- EvoMaster: 130 errors (+74 for LlamaRestTest)
- RESTler: 130 errors (+74 for LlamaRestTest)

### Fine-Tuning vs Quantization Impact

**Fine-Tuning Alone:**
- Modest improvements: +0.9 to +1.4 percentage points over base model
- 24.5 operations processed (+1.5 over base model's 23)

**8-bit Quantization with Fine-Tuning:**
- Substantial improvements: +5.1 to +6.8 percentage points
- 37.5 operations processed (+14.5 over base model)
- Best balance of effectiveness and efficiency

**4-bit Quantization:**
- Strong performance maintained
- 34.4 operations processed (+11.4 over base model)
- +4.7 to +5.0 percentage points coverage improvement

**2-bit Quantization:**
- Moderate improvements but limitations in complex scenarios
- 25.4 operations processed (+2.4 over base model)
- +2.5 to +4.1 percentage points coverage improvement

## Ablation Study Results

Impact of removing each component (percentage points decrease):

| Component Removed | Method | Branch | Line |
|-------------------|--------|---------|------|
| Full LlamaRestTest | 55.8% | 28.3% | 55.3% |
| Without LlamaREST-IPD | -9.2 | -6.2 | -8.4 |
| Without Server Response | -6.1 | -4.4 | -6.3 |
| Without Parameter Description | -5.9 | -1.4 | -4.5 |
| Without LlamaREST-EX | -3.7 | -4.5 | -3.0 |

**Key Findings:**
- LlamaREST-IPD (dependency detection) is the most critical component
- Server response analysis significantly contributes to effectiveness
- All components contribute meaningfully to overall performance

## Key Contributions

1. **First LM-based REST API Testing with Runtime Feedback:** Demonstrates that fine-tuned small language models can dynamically interact with servers during testing

2. **Fine-Tuning Effectiveness:** Shows that fine-tuning enables smaller models (8B parameters) to outperform much larger models (GPT-3.5 Turbo) in specialized tasks

3. **Quantization Benefits:** Demonstrates that quantization (especially 8-bit) improves both effectiveness and efficiency through faster inference and regularization effects

4. **Comprehensive Evaluation:** Extensive comparison across 12 real-world services with multiple state-of-the-art tools using code coverage, operation coverage, and fault detection metrics

5. **Open-Source Artifact:** Complete tool, fine-tuned models, training datasets, and evaluation results publicly available

## Technical Highlights

**Model Architecture:**
- Base: Llama3-8B with Llama2 tokenization
- Fine-tuning: QLoRA (Quantized Low-Rank Adaptation)
- Quantization: 2-bit, 4-bit, and 8-bit variants using llama.cpp

**Training Configuration:**
- Batch size: 8 (optimized for 40GB A100 GPU)
- Early stopping based on loss plateau
- Custom tokens for IPD rules and example values

**Inference Performance:**
- Fine-tuned model: 48.9 seconds per IPD
- 8-bit quantized: 36.9 seconds per IPD
- 4-bit quantized: 26.2 seconds per IPD
- 2-bit quantized: 26.1 seconds per IPD

## Practical Applications

**Advantages:**
- Runs on consumer-grade hardware (CPU-based systems supported)
- Significantly lower cost than GPT-based approaches
- No exposure of sensitive API information to external services
- Real-time adaptation to server feedback

**Use Cases:**
- Testing complex APIs with intricate parameter constraints
- Discovering undocumented parameter dependencies
- Generating domain-specific valid inputs (emails, IDs, coordinates)
- APIs with informative error messages

## BibTeX

```bibtex
@article{10.1145/3715737,
  author = {Kim, Myeongsoo and Sinha, Saurabh and Orso, Alessandro},
  title = {LlamaRestTest: Effective REST API Testing with Small Language Models},
  year = {2025},
  issue_date = {July 2025},
  publisher = {Association for Computing Machinery},
  address = {New York, NY, USA},
  volume = {2},
  number = {FSE},
  url = {https://doi.org/10.1145/3715737},
  doi = {10.1145/3715737},
  journal = {Proc. ACM Softw. Eng.},
  month = {jun},
  articleno = {FSE022},
  numpages = {24},
  keywords = {Automated REST API Testing, Language Models for Testing}
}
```