---
title: "Surgical Precision with AST"
date: 2026-06-23
tags: [code-agents, ast, kiro]
excerpt: "Why we moved Kiro's code editing from string matching to AST-based structural operations — and what it bought us in tokens, latency, and reliability."
---

We recently shipped an AST-based code navigation and editing engine in Kiro. The short version: targeting code by *structure* instead of *text* cut token usage by about 20% on our SWE-PolyBench feature-request set, and removed a whole class of brittle-edit failures. This post is my own summary; the full write-up is on the [Kiro blog](https://kiro.dev/blog/surgical-precision-with-ast/).

## The problem with text-based editing

Kiro IDE previously edited code with `readFile` and `strReplace`. Two costs compound quickly:

- **Tokens and latency.** Finding one function often means reading an entire file, spending most of the context budget on code the agent never touches.
- **Brittle string matching.** An edit needs an exact match. A difference in whitespace, formatting, or a nearby comment either fails outright or matches in several places. Each failure forces a re-read and another attempt — and the iterations add up.

## Editing by structure, not strings

The engine parses code into structured entities — functions, classes, imports — and operates on those directly.

**Reading** returns only what is needed: signatures, structure, or search results, rather than whole files. On a Java class example, the structural read used 545 tokens against 1,309 for the traditional approach — about 58% fewer.

**Writing** uses selectors like `ClassName.methodName` or `function:functionName` with four typed operations: `insert_node`, `replace_node`, `delete_node`, and `replace_in_node`. Because the edit names the target instead of quoting its surroundings, a TypeScript add-a-function example dropped from 361 tokens to 96 — about 73% fewer.

## What it bought us

On PolyBench50 (a subset of SWE-PolyBench):

| Metric | Traditional | AST-based | Improvement |
|---|---|---|---|
| LLM calls per task | 40.88 | 26.86 | 34.3% |
| Output tokens | 270,957 | 189,806 | 30.0% |
| Input tokens | 680,684 | 541,346 | 20.5% |

On an end-to-end feature request ("add third-party integrations to AWS Resource Explorer"), running time roughly halved (9m 20s to 4m 44s) and the two tool errors from the string-based run went to zero.

## Why this matters in production

The efficiency numbers are the headline, but the durable wins are about reliability:

- **Resilience** — a formatting change (spaces vs. tabs) doesn't break a structural edit.
- **Precision** — the operation targets exactly the intended element, not a similar-looking one elsewhere.
- **Maintainability** — edits stay valid as the surrounding code evolves.
- **Understanding** — parsing the AST gives the agent real structural context to reason over, not just a string to match.

This lands hardest on feature requests, which are the most common thing people ask Kiro to do and which usually span multiple files — exactly where string matching creates the most friction. The capability is live in production now.

For the full details, examples, and methodology, see the [Kiro blog post](https://kiro.dev/blog/surgical-precision-with-ast/).
