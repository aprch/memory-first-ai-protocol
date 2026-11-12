---
name: Model report
about: Share how a model (local or hosted) performed on the protocol checklist
title: "[MODEL] <model name> on <tool>"
labels: model-report
assignees: ''
---

## Setup

- **Model:** (e.g., qwen2.5-coder:14b, llama3.1:8b, claude-sonnet)
- **Quantization:** (e.g., Q4_K_M, Q8_0, fp16, n/a for hosted)
- **Context length used:** (e.g., 8192)
- **Runner:** (Ollama / LM Studio / llama.cpp / vLLM / hosted API)
- **Tool:** (Aider / Continue / Claude Code / other)
- **Hardware (optional):** (e.g., M2 Pro 32GB, RTX 4090)
- **`CLAUDE.md` length:** (lines)

## Checklist results

Tests from the README's "Testing a model against the protocol" section.

| # | Test | Pass / Partial / Fail | Notes |
|---|---|---|---|
| 1 | Rules loaded | | |
| 2 | Memory write | | |
| 3 | Learning loop | | |
| 4 | Resume ("Continue") | | |
| 5 | Recall | | |
| 6 | File safety | | |
| 7 | Working scope | | |
| 8 | Memory budget | | |

## Observations

What worked, what broke, and any prompt or config tweaks that helped.

## Snippets (optional)

Relevant bits of `CLAUDE.md` / `MEMORY.md` or transcript. Remove anything private first.
