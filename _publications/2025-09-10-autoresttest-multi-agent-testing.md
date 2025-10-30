---
title: "A Multi-Agent Approach for REST API Testing with Semantic Graphs and LLM-Driven Inputs"
collection: publications
category: conferences
permalink: /publication/2025-autoresttest-multi-agent-testing
excerpt: 'This paper presents AutoRestTest, the first black-box REST API testing tool to adopt a dependency-embedded multi-agent approach that integrates multi-agent reinforcement learning (MARL) with a semantic property dependency graph (SPDG) and Large Language Models (LLMs). AutoRestTest treats REST API testing as a separable problem where four specialized agents collaborate to optimize API exploration, achieving superior code coverage and fault detection compared to state-of-the-art tools.'
date: 2025-09-10
venue: 'Proceedings of the IEEE/ACM 47th International Conference on Software Engineering (ICSE 2025)'
paperurl: 'https://doi.org/10.1109/ICSE55347.2025.00179'
citation: 'Myeongsoo Kim, Tyler Stennett, Saurabh Sinha, and Alessandro Orso. 2025. A Multi-Agent Approach for REST API Testing with Semantic Graphs and LLM-Driven Inputs. In Proceedings of the IEEE/ACM 47th International Conference on Software Engineering (ICSE 2025). IEEE Press, 1409–1421.'
---

As modern web services increasingly rely on REST APIs, their thorough testing has become crucial. However, existing black-box REST API testing tools often focus on individual test elements in isolation (e.g., APIs, parameters, values), resulting in lower coverage and less effectiveness in fault detection.

**Resources:**
- [IEEE Xplore](https://doi.org/10.1109/ICSE55347.2025.00179)
- [arXiv Paper](https://arxiv.org/abs/2411.07098)
- [GitHub Repository](https://github.com/selab-gatech/AutoRestTest)

## The Problem

Current REST API testing tools face fundamental limitations:

**Isolated Optimization:**
- Each testing step (operation selection, dependency identification, parameter selection, value generation) is optimized in isolation
- No coordination between different testing decisions
- Suboptimal testing strategies with high invalid request rates

**Limited Dependency Discovery:**
- Rely on exact schema matching between inputs and outputs
- Struggle with incomplete or variant response formats
- Cannot discover hidden dependencies efficiently

**Context-Blind Value Generation:**
- Generate values without considering semantic meaning
- Fail to satisfy domain-specific constraints
- Poor handling of format-specific requirements (e.g., email patterns, IDs)

**Result:** Low coverage on complex services with many parameters, as shown in prior studies on LanguageTool, Genome Nexus, Spotify, and OhSome.

## Our Approach: AutoRestTest

We present **AutoRestTest**, the first black-box REST API testing tool to adopt a **dependency-embedded multi-agent approach** that integrates:

1. **Multi-Agent Reinforcement Learning (MARL):** Four specialized agents collaborate to optimize API exploration
2. **Semantic Property Dependency Graph (SPDG):** Reduces dependency search space using similarity-based modeling
3. **Large Language Models (LLMs):** Handle domain-specific value generation with semantic understanding

### Core Innovation: Separable Problem Decomposition

AutoRestTest treats REST API testing as a **separable problem** with four coordinated components:

```
API Agent → Dependency Agent → Parameter Agent → Value Agent
     ↓              ↓                  ↓               ↓
  Select       Identify          Choose          Generate
Operation   Dependencies      Parameters         Values
```

Unlike existing tools that optimize each step independently, AutoRestTest uses **value decomposition** to enable:
- **Decentralized execution:** Each agent selects actions independently
- **Centralized learning:** Q-value updates consider all agents' contributions
- **Coordinated optimization:** Agents learn to work together effectively

## Architecture and Workflow

### Initialization Phase

**1. OpenAPI Specification Parsing:**
- Extracts endpoint information
- Identifies parameters and schemas
- Prepares operation metadata

**2. SPDG Construction:**
- Creates directed graph with operations as nodes
- Computes semantic similarity using pre-trained word embeddings (GloVe)
- Adds weighted edges (0-1) representing potential dependencies
- Threshold: 0.7 similarity score (or top-5 if none exceed threshold)

**3. Agent Initialization:**
- Each agent initializes its Q-table
- Q-values start at 0 for all state-action pairs
- Epsilon-greedy strategy begins with ε = 1.0 (full exploration)

### Testing Execution Phase

**Iterative Testing Loop:**

1. **Operation Agent** selects next API operation to test
2. **Parameter Agent** determines parameter combinations
3. **Dependency Agent** identifies dependencies using SPDG
4. **Value Agent** generates parameter values (LLM/dependency/random)
5. **Request Generator** constructs and sends request (20% mutated)
6. **Response Handler** processes server response
7. **MARL Update** refines all agents' Q-tables using value decomposition
8. **SPDG Refinement** adjusts edge weights based on feedback

## The Four Specialized Agents

### 1. Operation Agent

**Responsibility:** Select next API operation to test

**State Model:** Operation availability only

**Action Space:** All API operations in specification

**Reward Structure:**
- +2: Server errors (5xx) → Continue testing this operation
- +1: Client errors (4xx, excluding 401/405) → Retest with different inputs
- -1: Successful responses (2xx) → Explore other operations
- -3: Authentication failures (401) → Move to other operations
- -10: Invalid methods (405) → Severely penalize

**Strategy:** Prioritize operations that reveal issues while avoiding systematically invalid requests

**Example:** For Market API's `/register` endpoint:
- Initial Q-value: 0
- After failures: Q-value increases → prioritize further testing
- After success: Q-value decreases → explore other endpoints

### 2. Parameter Agent

**Responsibility:** Select parameter combinations for operations

**State:** (operation_id, available_parameters, required_parameters)

**Action Space:** Possible parameter combinations (max 10 combinations)

**Reward Structure:**
- +2: Successful responses (2xx) → Valid combination
- -2: Client errors (4xx) → Invalid combination
- -1: Server errors (5xx) → Problematic combination
- 0: Unused combinations → Neutral (maintains initial Q-value)

**Key Feature:** Handles inter-parameter dependencies and prioritizes unused combinations with neutral Q-values over those with negative rewards

**Example:** Market API `/register` endpoint:
- State: {createCustomerUsingPOST, [email, name, password, links], [email, name, password]}
- Learns: (email, name, password) yields +2 reward
- Discovers: Adding optional `links` may cause issues

### 3. Value Agent

**Responsibility:** Generate and assign values to parameters

**State:** (operation_id, parameter_name, parameter_type, schema_constraints)

**Action Space:** Three data sources:
1. **Operation Dependency Values:** Reuse values from dependent operations
2. **LLM Values:** Generate using GPT-3.5 Turbo with few-shot prompting
3. **Random Values:** Type-based random generation

**Reward Structure:** Same as Parameter Agent (+2 for 2xx, -2 for 4xx, -1 for 5xx)

**Adaptive Learning:** Learns which data source works best for each parameter type and context

**Example:** Market API `/register` parameters:
- `email`: Q-values → LLM: 0.5, Random: 0.2, Dependency: -0.7
- `name`: Simpler format → Random values sufficient
- `password`: Pattern constraints → LLM values preferred

### 4. Dependency Agent

**Responsibility:** Manage and utilize operation dependencies from SPDG

**Q-table Structure:** Encodes SPDG edges categorized by:
- Parameter type (query or body)
- Target (parameters, body, or response)

**Key Features:**
- Higher Q-values indicate more reliable dependencies
- Recursively deconstructs response objects
- Permits random dependency queries during exploration
- Adds new edges when successful random dependencies discovered

**Dynamic Discovery:**
- Initial edges based on semantic similarity
- Runtime validation strengthens/weakens edge weights
- Discovers undocumented dependencies
- Adapts to incomplete response schemas

## Semantic Property Dependency Graph (SPDG)

### Construction

**Step 1: Node Creation**
- Each API operation becomes a node
- Nodes contain: operation ID, parameters, response schemas

**Step 2: Edge Creation (Semantic Similarity)**
- Compare parameter names (inputs) with response field names (outputs)
- Use cosine similarity with GloVe word embeddings
- Create edge if similarity score ≥ 0.7
- Weight edge with similarity score

**Step 3: Connectivity Guarantee**
- If operation has no edges above threshold
- Connect to top-5 most similar operations
- Ensures all operations have exploration paths

### Example SPDG

```
GET /users/{id} --0.9--> GET /orders/{user_id}
   (High similarity between id and user_id)

POST /register --0.85--> POST /carts
   (Discovered: returns user-related fields)

POST /carts --0.75--> GET /orders/{user_id}
   (Runtime discovery: cart_id useful for queries)
```

### Dynamic Refinement

**Phase 1: Initial SPDG**
- Based on semantic similarity only
- May miss some valid dependencies
- May include invalid assumptions

**Phase 2: Runtime Discovery**
- Agents explore random dependencies (during ε-greedy exploration)
- Successful random dependencies added as new edges
- Undocumented response fields incorporated

**Phase 3: Continuous Refinement**
- Successful dependencies → increase edge weight
- Failed dependencies → decrease edge weight
- Heavily penalized edges → effectively removed

**Benefits:**
- Reduces O(n²) dependency search to manageable subset
- Discovers hidden dependencies not in specification
- Adapts to incomplete or variant response schemas
- More precise than control-flow heuristics (like Razor)

## Multi-Agent Reinforcement Learning (MARL)

### Q-Learning with Value Decomposition

**Core Principle:** Joint Q-value decomposed additively across agents

```
Q(s, a) = Σᵢ Qᵢ(s, aᵢ)
```

where:
- `Q(s, a)`: Joint Q-value for all agents
- `Qᵢ(s, aᵢ)`: Individual agent i's Q-value
- `s`: Shared state
- `aᵢ`: Agent i's action

### Action Selection (Decentralized)

Each agent independently uses **ε-greedy strategy:**

```
With probability ε: Select random action (exploration)
With probability 1-ε: Select action with highest Q-value (exploitation)
```

**Epsilon Decay:**
- Start: ε = 1.0 (full exploration)
- End: ε = 0.1 (mostly exploitation)
- Strategy: Linear decay over testing duration

### Policy Update (Centralized)

**Temporal-Difference Update Rule:**

```
Qᵢ(s, aᵢ) ← Qᵢ(s, aᵢ) + α[r + γ max Σᵢ Qᵢ(s', a'ᵢ) - Σᵢ Qᵢ(s, aᵢ)]
```

where:
- α = 0.1 (learning rate)
- γ = 0.9 (discount factor)
- r: Reward from server response
- s': Next state
- a'ᵢ: Next action for agent i

**Key Advantage:** Each agent updates Q-values considering all agents' contributions through value decomposition

### Hyperparameters

Based on ARAT-RL and related work:
- Learning rate α = 0.1
- Discount factor γ = 0.9
- Initial epsilon ε = 1.0
- Final epsilon ε = 0.1

## LLM-Driven Value Generation

**Model:** GPT-3.5 Turbo with temperature 0.8

**Few-Shot Prompting Strategy:**
- Provides parameter name, type, and constraints
- Includes few representative examples
- Generates contextually appropriate values

**Value Generation Flow:**

1. **Value Agent** selects LLM as data source
2. **Prompt Construction:** 
   - Parameter context (name, type, description)
   - Schema constraints (pattern, format, range)
   - Few-shot examples
3. **LLM Generation:** Creates values satisfying constraints
4. **Validation:** Checks against schema requirements
5. **Feedback:** Updates Q-value based on server response

**Example Prompts:**

**For email parameter:**
```
Generate a valid email address following pattern:
^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]+$

Examples:
- john.doe@example.com
- user_123@company.org

Generate:
```

**For Spotify playlist_id:**
```
Generate a valid Spotify playlist ID (22 characters, 
alphanumeric with specific constraints).

Examples:
- 37i9dQZF1DXcBWIGoYBM5M
- 3cEYpjA9oz9GiPac4AsH4n

Generate:
```

## Request Generator and Mutator

### Request Construction

**Input from Agents:**
1. Operation (from Operation Agent)
2. Parameters (from Parameter Agent)
3. Parameter values (from Value Agent)
4. Dependencies (from Dependency Agent)

**Construction Process:**
1. Build HTTP request headers
2. Construct URL with path parameters
3. Add query parameters
4. Build request body (if applicable)
5. Apply authentication if needed

### Mutation Strategy

**Purpose:** Generate invalid requests to uncover unexpected behaviors (500 errors)

**Mutation Rate:** 20% of requests (following ARAT-RL)

**Mutation Types:**
- Parameter type changes
- Value mutations
- Header modifications (e.g., invalid content-type)
- Boundary value testing

**Rationale:** State-of-the-art tools employ similar strategies, proven effective for finding server-side issues

## Evaluation Results

Evaluated on **12 real-world REST services** comparing against four state-of-the-art tools (RESTler, EvoMaster, MoRest, ARAT-RL) using RESTGPT-enhanced specifications.

### Code Coverage (Open-Source Services)

| Metric | AutoRestTest | ARAT-RL | EvoMaster | MoRest | RESTler |
|--------|--------------|---------|-----------|---------|---------|
| **Method Coverage** | **58.3%** | 42.1% | 43.1% | 31.5% | 34.7% |
| **Line Coverage** | **58.3%** | 44.0% | 44.1% | 33.4% | 34.6% |
| **Branch Coverage** | **32.1%** | 19.8% | 20.5% | 12.7% | 11.4% |

**Coverage Gains:**
- Method: 15.2 to 26.8 percentage points
- Line: 14.2 to 24.8 percentage points
- Branch: 11.6 to 20.7 percentage points

### Operation Coverage (Online Services)

**Total Operations Exercised:**

| Service | AutoRestTest | ARAT-RL | EvoMaster | MoRest | RESTler |
|---------|--------------|---------|-----------|---------|---------|
| FDIC | 6 | 6 | 6 | 6 | 6 |
| OhSome | **12** | 0 | 0 | 0 | 0 |
| Spotify | **7** | 5 | 4 | 4 | 3 |
| **Total** | **25** | 11 | 10 | 10 | 9 |

### Fault Detection (500 Errors)

| Service | AutoRestTest | ARAT-RL | EvoMaster | MoRest | RESTler |
|---------|--------------|---------|-----------|---------|---------|
| Features Service | 1 | 1 | 1 | 1 | 1 |
| Language Tool | 1 | 1 | 1 | 0 | 0 |
| REST Countries | 1 | 1 | 1 | 1 | 1 |
| Genome Nexus | 1 | 1 | 0 | 1 | 0 |
| Person Controller | 8 | 8 | 8 | 8 | 3 |
| User Management | 1 | 1 | 1 | 1 | 1 |
| Market | 1 | 1 | 1 | 1 | 1 |
| Project Tracking | 1 | 1 | 1 | 1 | 1 |
| YouTube | 1 | 1 | 1 | 1 | 1 |
| FDIC | 6 | 6 | 6 | 6 | 6 |
| **OhSome** | **20** | 12 | 0 | 0 | 0 |
| **Spotify** | **1** | 0 | 0 | 0 | 0 |
| **Total** | **42** | 33 | 20 | 20 | 14 |

**Key Achievement:** Only tool to detect errors in Spotify (615 million users) and discovered 20 errors in OhSome (verified and fixed).

### Real-World Impact Examples

**Example 1: OhSome Service**
- **Sequence:** POST `/elements/area/ratio` with `filter2=node:relation`
- **SPDG Discovery:** Identifies similarity between `filter2` and `filter` parameters
- **Error Trigger:** GET `/users/count/groupBy/key` with `filter=node:relation`
- **Result:** 500 Internal Server Error (should be 4xx client error)
- **Other Tools:** Miss correlation due to naming differences

**Example 2: Spotify API**
- **Challenge:** `playlist_id` requires specific 22-character format
- **LLM Generation:** Creates valid Spotify playlist IDs
- **Success:** GET `/playlists/{playlist_id}/tracks` works
- **Discovery:** Retrieved ISRC used in subsequent operation
- **Error Trigger:** Hidden dependency conflict causes 500 error
- **Other Tools:** Cannot generate valid playlist IDs, miss entire operation chain

### Ablation Study

Impact of removing each component (percentage points decrease):

| Component Removed | Method | Line | Branch |
|-------------------|--------|------|---------|
| Full AutoRestTest | 58.3% | 32.1% | 58.3% |
| **Without Q-Learning** | 45.6% (-12.7) | 18.2% (-13.9) | 45.8% (-12.5) |
| **Without SPDG** | 46.7% (-11.6) | 18.7% (-13.4) | 47.6% (-10.7) |
| **Without LLM** | 47.4% (-10.9) | 19.3% (-12.8) | 45.8% (-12.5) |

**Key Findings:**
- **Q-Learning** contributes most (12.5-13.9% decrease)
- **SPDG** significantly reduces dependency search space (10.7-13.4% decrease)
- **LLM** enables semantic value generation (10.9-12.8% decrease)
- **All components essential** for optimal performance

**Spotify Example:** Without SPDG, only 5 operations covered vs. 7 with SPDG

## Key Advantages Over State-of-the-Art

**1. Coordinated Multi-Agent Optimization:**
- Value decomposition enables agents to learn collaboratively
- Superior to isolated, independent optimization
- Agents converge toward optimal joint strategy

**2. Efficient Dependency Discovery:**
- SPDG reduces O(n²) search to manageable subset
- Runtime discovery captures undocumented dependencies
- More precise than exact schema matching

**3. Context-Aware Value Generation:**
- LLMs understand semantic requirements
- Generate domain-specific values (emails, IDs, patterns)
- Outperforms random or keyword-based generation

**4. Adaptive Learning:**
- Continuous refinement based on feedback
- Balances exploration and exploitation
- Discovers optimal parameter combinations

**5. Comprehensive Coverage:**
- 12-27% higher method coverage
- 14-25% higher line coverage
- 12-21% higher branch coverage

**6. Superior Fault Detection:**
- 27% more errors than ARAT-RL
- 110% more errors than EvoMaster/MoRest
- 200% more errors than RESTler
- Only tool to find Spotify error

## Benchmark Services

Evaluated on 12 diverse services:

**Open-Source (with code coverage):**
- Features Service (1,688 LoC, 18 ops)
- Language Tool (113,170 LoC, 2 ops)
- REST Countries (1,619 LoC, 22 ops)
- Genome Nexus (22,143 LoC, 23 ops)
- Person Controller (601 LoC, 12 ops)
- User Management (2,805 LoC, 22 ops)
- Market Service (7,945 LoC, 13 ops)
- Project Tracking System (3,784 LoC, 67 ops)
- YouTube Mock (242 LoC, 21 ops)

**Online (operation coverage only):**
- FDIC (6 ops)
- Spotify (12 ops)
- OhSome API (122 ops)

## Tool Comparison

| Tool | Approach | Dependency Model | Value Generation | Learning |
|------|----------|------------------|------------------|----------|
| RESTler | Grammar-based fuzzing | Exact schema matching | Type-based | None |
| EvoMaster | Evolutionary algorithms | Producer-consumer | Search-based | Evolutionary |
| MoRest | Model-based | RESTful Property Graph | Graph-guided | None |
| ARAT-RL | Single-agent RL | Frequency-based | Random/Spec | Q-learning |
| **AutoRestTest** | **Multi-agent RL** | **SPDG + Runtime** | **LLM + Adaptive** | **MARL** |

## Key Contributions

1. **First Multi-Agent REST API Testing Tool:** Novel application of MARL with value decomposition for coordinated test generation

2. **Semantic Property Dependency Graph:** Efficient dependency modeling using similarity-based graph with runtime refinement

3. **Integrated LLM Value Generation:** Context-aware parameter value generation using few-shot prompting

4. **Comprehensive Evaluation:** Extensive comparison on 12 services demonstrating 12-27% coverage improvements

5. **Real-World Impact:** Detected and reported bugs in actively maintained services (Spotify, OhSome)

6. **Open-Source Artifact:** Complete tool, benchmark services, and experimental results publicly available

## BibTeX

```bibtex
@inproceedings{10.1109/ICSE55347.2025.00179,
  author = {Kim, Myeongsoo and Stennett, Tyler and Sinha, Saurabh and Orso, Alessandro},
  title = {A Multi-Agent Approach for REST API Testing with Semantic Graphs and LLM-Driven Inputs},
  year = {2025},
  isbn = {9798331505691},
  publisher = {IEEE Press},
  url = {https://doi.org/10.1109/ICSE55347.2025.00179},
  doi = {10.1109/ICSE55347.2025.00179},
  booktitle = {Proceedings of the IEEE/ACM 47th International Conference on Software Engineering},
  pages = {1409–1421},
  numpages = {13},
  keywords = {multi-agent reinforcement learning for testing, automated REST API testing},
  location = {Ottawa, Ontario, Canada},
  series = {ICSE '25}
}
```