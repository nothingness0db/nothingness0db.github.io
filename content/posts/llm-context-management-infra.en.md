---
title: "Dynamic Context Management Infrastructure for LLMs"
author: "Eira Hazel"
date: 2026-04-02T02:24:00+08:00
draft: false
tags:
  - LLM
  - Context Management
  - Semantic Compression
  - System Design
  - AI Engineering
categories:
  - Tech
---

My original idea was extremely simple: optimize at the model layer by storing sparse feature vectors directly as the compressed representation, without restoring them into text. During downstream consumption, these feature vectors would be injected back into the corresponding layers of the same model, skipping token decoding and re-encoding. Alright, that was the model-layer approach. Obviously, what I’m writing here isn’t that, because I can’t pull that off in less than a month. But I came up with a practical alternative that works now and is worth exploring. Below is an AI-generated summary of my chat records with alam.

> Design Overview · Draft

---

## Abstract

This article traces a design exploration that evolved from a "text semantic compressor" to an "LLM context management infrastructure layer." It covers core topics including model selection, data construction, engineering architecture, retrieval mechanisms, and semantic structure maintenance, ultimately framing the problem as an engineering task centered on a dynamic semantic database rather than a pure model-training task.

---

## 1. Starting Point: Semantic Text Compression

The initial goal was to build a compressor that preserves core semantics while pruning verbosity. Unlike keyword extraction or abstractive summarization, semantic compression demands that the compressed text remain semantically equivalent to the original, serving as a higher-density alternative input.

**Initial Plan**: Fine-tune a small model to replace expensive SOTA LLM calls. Base model: Qwen2.5-1.5B-Instruct. Training data distilled from a larger model. Training stack: LLaMA-Factory + QLoRA on a single consumer GPU (3090/4090).

---

## 2. Pipeline Design: Chunking + Semantic State Passing

To handle long texts, a chunked compression pipeline was proposed:

- Traditional methods handle coarse-grained chunking
- The small model compresses each chunk
- Cross-chunk coherence is maintained via "semantic prompts" that carry context state

The core challenge: when chunk boundaries don't align with semantic units, semantic prompts have limited compensatory power. Meanwhile, the representation of semantic state (summaries, entity lists, structured descriptions) directly affects compression quality and training data design.

---

## 3. Comparison with SOTA

Compared to SOTA models (GPT-4o, Claude 3.5 Sonnet, etc.), this approach lags in general compression quality. The root cause is that SOTA models can grasp the full document holistically, whereas chunked approaches suffer from localized information loss.

However, **the choice of evaluation framework changes the competitive landscape**. When the target use case shifts from "human-readable summary" to "downstream LLM input," SOTA's advantage shrinks dramatically—existing SOTA models are not specifically optimized for "downstream task performance after compression." If the training objective is tuned directly for downstream accuracy, a small model can theoretically surpass general SOTA on this narrow task.

---

## 4. Real Need: Long Context Compression for LLMs

The real scenario is anchored in context overflow for Claude Opus-level models inside Claude Code. Complex programming tasks quickly exhaust the context window, and existing solutions (calling SOTA for compression) are costly and mediocre.

**Key Insight**: The root problem of context overflow is not length, but noise—the conversation history is full of discarded plans, repeated error messages, and irrelevant intermediate states. Thus, the core value of a compressor is **denoising**, not merely shortening.

Moreover, if the compressed context lets the downstream model reduce token consumption while improving task completion rate, the positioning of the solution upgrades from "low-cost alternative" to "higher-quality context management layer."

---

## 5. An Engineering Task, Not a Model Task

As the scenario deepened, the nature of the problem shifted. Model fine-tuning is just one operator in the whole system. The real engineering complexity lies in:

- How to plug into Claude Code's context pipeline
- Differentiated processing logic for two content types (conversation history vs. code files)
- Coordination layer design between the two pipelines
- Latency budget control across the entire chain

**Conversation history** follows temporal decay logic: early exploration can be heavily compressed, while recent actions and current error contexts must be preserved intact.

**Code files** follow task-relevance logic: they are not cropped by time, but by keeping interface definitions and key functions relevant to the current task while folding irrelevant implementation details.

The two pipelines are not independent; the semantic state of the conversation history must back-propagate to guide the compression granularity of code files.

---

## 6. Context Database: The Emergence of the Core Abstraction

Expanding the "compressor" into a "context database" is the most critical conceptual leap in this discussion.

The fundamental flaw of existing LLM contexts is their linear structure, volatility, and lack of organization—four semantic layers (facts, action records, intent, state) are flattened into the same sequence without differentiated management.

Core primitives of the context database:

| Operation | Semantics |
|-----------|-----------|
| **Write** | Content + task context → compression + semantic vector + metadata (source, time, importance weight) |
| **Retrieve** | Current task → relevance ranking → dynamically assemble context window within token budget |
| **Update** | When a new version of the same file arrives, the old version is down-weighted or invalidated, not simply overwritten |
| **Forget** | Long-unretrieved content is further compressed or archived, not deleted |

The essential difference from existing solutions (RAG, MemGPT, etc.) is that this approach unifies compression, indexing, retrieval, and lifecycle management at the **infrastructure layer**, rather than making localized optimizations at the application layer.

---

## 7. Retrieval: The Hardest Part

The fundamental difficulty of retrieval is that **relevance is semantic, not structural**. Function B may be tightly coupled to Function A in ways that never appear in the call graph, instead being implicit in business logic. Rules cannot discover such relationships, while real-time LLM judgment introduces latency and cost issues.

**Way out**: Shift real-time semantic judgment to **offline precomputation at write time**.

Concrete path:

1. Static analysis builds the call graph (rule layer)
2. An LLM scans the code to identify semantically strong couplings not reflected in the call graph, supplementing edges (semantic enhancement layer, executed offline)
3. Retrieval follows graph traversal, diffusing from the current task node by weight, truncated by token budget

The LLM's role thus shifts to **asynchronously maintaining the semantic relationship graph**, rather than participating in every retrieval decision in real time.

---

That's about as far as the discussion with my friend went for now. New content is still experimental, so it's not ready to publish yet.
