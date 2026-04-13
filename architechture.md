# mirkwood architecture

A technical description of the Mirkwood pipeline. Capture, ingestion, analyst layer. Why the architecture works, why the alternatives don't, and why none of its components are exotic.

This document describes patterns and decisions. It does not describe hardware part numbers, firmware, schema definitions, tuning parameters, or anything else that constitutes a reproduction path. Those are withheld by design. See `README.md` for the scope boundary.

---

## Overview

Mirkwood has three layers. Capture, storage, analyst. Each one is built from commodity components. The novelty is not in any single layer. The novelty is that the three layers, pointed at the same target at the same time, collapse per-channel pseudonymity in a way that each layer individually cannot.

The capture layer is a physical containment lab. The storage layer is a unified spatiotemporal event store. The analyst layer is a schema-first natural-language-to-SQL pipeline running on a commodity LLM.

The third layer is the one this document focuses on. It is the load-bearing piece, and it is the piece that is most frequently overbuilt in comparable systems.

## Capture layer

Real silicon, real protocol frames, real bands, contained by RF dummy loads that drop effective transmission range from miles to inches. Commodity microcontrollers impersonate vendor device classes by spoofing identifier-space prefixes. Offline reference databases provide validation. Every identifier in the lab is fabricated. The envelope is physical, not declarative.

Architectural decisions worth naming:

Containment is enforced by the physics of the test setup, not by software policy. This is categorically different from "the code filters out real data." A spectrum analyzer in the room can, in principle, verify the containment. A policy layer cannot be verified the same way.

Targets generate real frames rather than replayed captures. Replayed captures carry real metadata and are never fully synthetic regardless of how they are edited. Fabricated frames from scratch cannot carry residual real-world identifiers because none ever existed.

The entire capture layer runs on commodity hardware at a cost that is the core of the accessibility argument. The specific number is in the README. The specific parts are not in either document.

## Storage layer

All captured events land in a unified spatiotemporal event store with a common schema across channels. One row per event. Every row has a timestamp, a coordinate, a channel type, and a set of channel-specific fields. The design goal is a single table the analyst layer can reason about without needing to know which channel produced which row.

This is the cheapest layer to build and the one that gets the least attention in comparable systems. That is a mistake. The correlation methodology is fundamentally a *join*. If the storage layer does not present the channels as joinable, the analyst layer has to do that work, and the analyst layer is the wrong place for it.

The store is BigQuery. It could be anything with a SQL interface and a columnar engine. The choice is not interesting.

## Analyst layer

This is the piece worth documenting in detail.

The analyst layer accepts natural-language questions from a human operator, generates SQL against the storage layer, executes the SQL, and renders the results. The implementation is a few hundred lines of JavaScript in an Electron shell. It uses no retrieval-augmented generation, no vector database, no embedding index, no agent framework, no tool-calling layer, no chain-of-thought prompting, no fine-tuning. It uses one LLM call per question.

### The pattern

The pipeline is four steps.

**1. Schema introspection on connect.** When the analyst layer connects to the store, it lists every table in the target dataset, fetches each table's schema metadata, and caches the result in memory. Field names and types only. No sample rows, no statistics, no values. This cache is the only context the LLM ever sees about the data.

**2. Schema injection as literal prompt text.** When the user asks a question, the cached schema is serialized into the system prompt as plain text. Every table, every column, every type. The schema is not retrieved, embedded, chunked, or ranked. It is pasted. Along with it: a short set of SQL dialect rules, a row limit, and a structured escape hatch.

**3. Naked SQL output.** The model is instructed to return only the SQL query. No markdown fences, no explanation, no JSON wrapper, no preamble. If the question cannot be answered from the schema, the model is instructed to return the literal string `CANNOT_ANSWER`. The output is checked for that sentinel before any execution attempt.

**4. Direct execution.** The SQL is handed to the storage layer without post-processing, without validation beyond the sentinel check, without rewriting, without a planner. The result set is serialized and rendered to the user. The generated SQL is shown alongside the result so the operator can verify it.

That is the whole pipeline. There is no step five.

### Why this works where heavier stacks fail

Every piece of this pipeline is a deliberate rejection of a pattern that is conventional in the current LLM tooling ecosystem. Each rejection is load-bearing.

**No RAG because the schema fits.** Retrieval-augmented generation exists because relevant context is too large to fit in a prompt. A cross-modal surveillance schema is not large. It is a few dozen tables at most, a few hundred columns total, and the model needs all of them available for every query because the correlation questions are fundamentally joins across tables. Embedding the schema and retrieving "relevant" chunks would actively hurt the model's ability to reason about joins, because the joins are what the relevance filter would cut. The correct move is to hand the model the whole schema every time. Flash-tier context windows make this free.

**No vector database because there is no corpus to search.** The data the model reasons *about* is the schema. The data the model operates *on* is the live store. Neither of those is a corpus that benefits from semantic search. A vector database in this pipeline would be infrastructure in search of a problem.

**No agent framework because the task is single-step.** The question-to-SQL transformation is one LLM call. Adding a planner, a tool-calling layer, or a reflection loop introduces failure modes (agents that second-guess themselves into worse SQL, planners that fragment one query into six) without introducing capability. The narrow scope of the task is the thing that makes the naive approach correct.

**No chain-of-thought because the output format forbids it.** Chain-of-thought prompting works by giving the model room to reason in natural language before committing to an answer. The output contract here is "nothing but SQL." The model does its reasoning internally and commits. Empirically, on Flash-tier models against well-structured schemas, this is sufficient for the question classes the system is designed for. The generated SQL is shown to the user for verification precisely because the reasoning is hidden.

**No fine-tuning because generic capability is the point.** A fine-tuned model would be a piece of infrastructure that has to be maintained, versioned, and re-trained when the schema changes. A generic model with a schema-in-prompt approach adapts to schema changes the next time the system reconnects to the store. The price of this adaptability is roughly zero.

**`CANNOT_ANSWER` as a structured escape hatch.** The single most important line in the system prompt. Without it, the model will hallucinate SQL against columns that do not exist, or misinterpret an out-of-scope question as an in-scope one. With it, and with the sentinel check before execution, out-of-scope questions route to a polite refusal instead of a runtime error or a wrong answer. The cost is one string comparison. The alternative is validating generated SQL against the schema before execution, which is a parser and a semantic checker and a maintenance burden. The sentinel moves that entire problem into the prompt.

### What the pipeline does not do

It does not write data. It does not modify the schema. It does not execute user-provided SQL. It does not accept structured queries at all — every query originates as natural language and is generated by the LLM, which means the attack surface is the prompt, not the SQL layer. The BigQuery credentials it holds are read-only by design. These are not decorative constraints; they are what keep the blast radius of a prompt injection at "bad query" instead of "modified dataset."

## Why this matters for the threat model

The analyst layer is the piece of Mirkwood that is most relevant to the threat the project is describing, and it is the piece that most clearly illustrates why the threat is underappreciated.

The capture layer has always been available to operators with enough budget. What was missing, historically, was the analyst. Turning raw cross-modal telemetry into answers used to require an analyst who could write SQL against an unfamiliar schema, knew the domain, and could iterate on queries until the correlation surfaced. That is a skilled labor bottleneck, and skilled labor bottlenecks are what used to make this class of surveillance expensive enough that only well-resourced operators could sustain it.

The schema-first LLM pattern removes the bottleneck. Not by being impressive. By being trivial. A few hundred lines of JavaScript, one API key, and a willingness to let the model see the whole schema at once. That is the entire engineering lift. The same pattern is already deployed in the wild answering questions about call queues, sales pipelines, support tickets, HR metrics — every enterprise dataset that someone got tired of writing SQL for. It is commoditized technology, being used for commoditized reasons, and it happens to work unchanged when pointed at a fused surveillance corpus.

That is the threat. Not that someone invented anything. That every piece of the pipeline is already sitting on a workbench somewhere, normalized, documented, and one configuration change away from working. The research contribution is making that legibility explicit. The engineering contribution is demonstrating that none of it is hard.

## What is deliberately not documented

Specific prompt text beyond the structural elements described above. The schema of the fused store. The SQL dialect rules passed to the model. The exact question classes the system handles reliably versus unreliably. The failure modes of the `CANNOT_ANSWER` sentinel and how often it fires. The latency characteristics. The cost per query. Any calibration data against realistic scenarios.

These are withheld for the same reason the capture-side BOM is withheld. The architectural pattern is the publishable contribution. The tuning is the reproduction path.
