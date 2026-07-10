# Agentic AI, Drawn to the Metal

A single-page **field atlas of 15 hand-drawn SVG schematics** covering the low-level architecture of agentic AI systems — from the token substrate up through the eval and observability plane.

**Open it:** just serve the folder and open `index.html` (no build step, no dependencies):

```bash
python3 -m http.server 8000
# → http://localhost:8000/index.html
```

## The 15 figures

| Part | Figures |
|---|---|
| **I — The Substrate** | 01 Prompt → token → inference · 02 Inference serving path · 03 Context window assembly |
| **II — The Agent Core** | 04 The agent loop · 05 Anatomy of a tool call · 06 MCP architecture · 07 RAG pipeline · 08 Memory architecture |
| **III — Scale & Safety** | 09 Multi-agent orchestration · 10 Guardrails & human-in-the-loop · 11 Failure modes → recovery |
| **IV — Evals & Observability** | 12 The eval harness · 13 LLM-as-judge, calibrated · 14 Observability plane · 15 The data flywheel |

Each figure ships with **Design notes** (the decisions that matter) and **What breaks** (the failure modes that bite in production).

## How it's built

- One self-contained `index.html` — inline CSS, inline SVG, ~40 lines of vanilla JS (scrollspy + reveal). No frameworks, no chart libraries.
- A shared SVG visual language: teal = data path, violet = control/loop, amber = eval & feedback signal, red = failure/blocked, dashed = async/optional.
- Design tokens match [ljafri.me](https://ljafri.me) (slate dark surfaces, Inter + JetBrains Mono), so it can be deployed under the portfolio (e.g. `ljafri.me/agent-architectures/`) as-is.
