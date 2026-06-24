---
title: "Reading Notes: Coding-Agent Harnesses, Evals, and RL"
date: 2026-06-24
tags: [code-agents, harness, evaluation, reinforcement-learning]
excerpt: "A few recent papers I have been reading on agent process discipline, workflow migration, multi-agent code generation, and RL for code."
---

A few recent papers I have been reading, grouped by theme. Notes are my own; the work is the authors'.

## Process discipline and evaluation

**[RigorBench: Benchmarking Engineering Process Discipline in Autonomous AI Coding Agents](https://arxiv.org/abs/2606.22678)** measures *how* an agent codes rather than only whether tests pass. It scores five process pillars (planning fidelity, verification coverage, recovery efficiency, abstention quality, atomic transition integrity) across 30 tasks and four systems in a with/without design, aggregated into a single RigorScore.

This is close to my own interest in trajectory-based diagnosis. In [Coherence Collapse](https://arxiv.org/abs/2603.24631) we found that Pass@1 hides *why* capable models fail, and that many failures reach the right code and then overwrite or thrash it. RigorBench's recovery-efficiency and atomic-transition-integrity pillars are process-level measures of the same phenomenon. As an early benchmark the scale is modest (30 tasks) and the results are tied to a single discipline framework, so I read the headline deltas as directional and look forward to a larger follow-up.

## Harness and workflow engineering

**[Toward Self-Evolution-Ready Workflow Harnesses](https://arxiv.org/abs/2606.24598)** applies a Strangler-Fig migration pattern to refactor legacy "LLM + script" production workflows into modular, typed, auditable stages, with an A/B/C convertibility taxonomy: A is already stage-shaped (wrap and toolify), B has logic tangled in prompts (decompose first), C is a monolithic prompt (needs a code-first rewrite). The case study migrates a real content workflow with zero business-logic change.

I like that it starts from the realistic situation of existing production pipelines rather than greenfield agents. The convertibility test (can each step run independently on a typed input and output?) is a useful lens for deciding whether a harness can be decomposed. The authors are clear that the self-evolution piece is an early signal rather than a validated learning result, which I appreciate.

**[CodeTeam: An LLM-Powered Multi-Agent Framework for Repository-Level Code Generation](https://arxiv.org/abs/2606.22082)** uses a four-role pipeline: architect agents draft competing software design sketches, a CTO agent scores and selects one as a machine-checkable contract, developer agents implement files under a dependency-aware scheduler with file ownership, and a QA agent generates tests and routes failures back to owners.

The part I find most interesting is the design sketch as an executable contract over a typed interface spec, which is adjacent to my [CodeStruct](https://arxiv.org/abs/2604.05407) work on agents over structured action spaces. The structured commit metadata (target file, modified symbols, affected dependents) is also a clean trajectory-logging pattern. The baselines are adapted to isolate the workflow, so I read the reported margins as directional rather than absolute.

## RL for code

**[P4IR: RL to improve LLM-based automated code compliance](https://arxiv.org/abs/2606.22402)** is a two-stage SFT then GRPO pipeline that generates code skeletons from building regulations. The reward combines a Jaccard similarity over hierarchical signatures with explicit penalties for invalid syntax and for trivially short output.

This is a concrete worked example of the pipeline on my own study roadmap (SFT then RL on a coding task) with a fully spelled-out, non-LLM-judged reward. The reward design is the lesson for me: an accuracy term plus explicit anti-gaming penalties is a clean template for the kind of execution- or structure-signal grader I want to build. The domain is narrow, but the method transfers.

## Community

Also worth a look this week: a widely-shared [guide to setting up a local coding agent on macOS](https://ikyle.me/blog/2026/how-to-setup-a-local-coding-agent-on-macos), and an ongoing discussion on [whether agents.md files actually help](https://twitter.com/rasbt/status/2063649136323252397).
