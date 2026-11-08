# Agentic AI, Drawn to the Metal

**A field atlas of 15 hand-drawn SVG schematics covering how agentic AI systems actually work in production**, from the token substrate up through the eval harness and observability plane.

Live at [ljafri.me/agent-architectures](https://ljafri.me/agent-architectures/).

## Why this exists

Most agent diagrams stop at "LLM + tools." They skip the parts that decide whether a system survives contact with real traffic: how the context window is assembled and budgeted, where permissions and retries can actually live, what a tool call looks like on the wire, how the loop terminates, and the eval and observability machinery that makes any of it trustworthy.

This atlas is the reference I wanted when I started designing and reviewing agentic systems for a regulated fintech. Every figure is drawn at the level where architecture decisions get made, and every figure ships with two columns:

- **Design notes**: the decisions that matter and why.
- **What breaks**: the failure modes that bite in production, drawn from building and operating these systems rather than from tutorials.

The figures are kept current. When a spec or convention moves (the MCP 2026-07-28 stateless revision, the OpenTelemetry GenAI semantic conventions), the affected figure is redrawn and the change is logged below.

## Three ways in

| View | For | File |
|---|---|---|
| **Atlas** | Working through all 15 figures with design notes and failure modes | `index.html` |
| **Speedrun** | 15 one-idea cards with a check-off progress bar, about 5 minutes | `speedrun.html` |
| **One-glance flow** | The whole architecture as one traceable line, five beats, animated | `flow.html` |

Open any of them directly, or serve the folder:

```bash
python3 -m http.server 8000
# → http://localhost:8000/index.html
```

No build step, no dependencies.

## The 15 figures

| Part | Figures |
|---|---|
| **I — The Substrate** | 01 Prompt → token → inference · 02 Inference serving path · 03 Context window assembly |
| **II — The Agent Core** | 04 The agent loop · 05 Anatomy of a tool call · 06 MCP architecture · 07 RAG pipeline · 08 Memory architecture |
| **III — Scale & Safety** | 09 Multi-agent orchestration · 10 Guardrails & human-in-the-loop · 11 Failure modes → recovery |
| **IV — Evals & Observability** | 12 The eval harness · 13 LLM-as-judge, calibrated · 14 Observability plane · 15 The data flywheel |

Figures cross-reference each other (FIG·05 validates what FIG·04 emits; FIG·14's traces become FIG·12's eval cases; FIG·15 closes the loop). Part IV is the section most teams build last and need first.

## How it's built

- One self-contained HTML file per view: inline CSS, inline SVG, a few dozen lines of vanilla JS (scrollspy, reveal, progress state in `localStorage`). No frameworks, no chart libraries.
- A shared SVG visual language: teal = data path, violet = control/loop, amber = eval & feedback signal, red = failure/blocked, dashed = async/optional.
- Design tokens match [ljafri.me](https://ljafri.me) (slate dark surfaces, Inter + JetBrains Mono), so it deploys under the portfolio as-is.

## Revision log

- **Nov 2026**: FIG 06 redrawn for MCP revision 2026-07-28 (stateless protocol: no `initialize` handshake, no `Mcp-Session-Id`, version and capabilities in `_meta`, `server/discover`, multi-round-trip requests, `Mcp-Method`/`Mcp-Name` headers; roots, sampling, logging deprecated; `resources/subscribe` removed). FIG 14 span names aligned to the OpenTelemetry GenAI semantic conventions (`invoke_agent`, `chat`, `execute_tool`, `gen_ai.evaluation.result`). FIG 05 adds programmatic tool calling; FIG 08 adds provider memory tools, server-side compaction, and skills as the procedural store.
- **Jul 2026**: initial 15 figures, speedrun and one-glance flow views. Split out of a personal monorepo into this repository.

## Reuse

Figures and text © Laraib Jafri. Reuse with attribution and a link back is welcome; open an issue for anything beyond that.

## Author

Laraib Jafri · [ljafri.me](https://ljafri.me) · architect and engineer building agentic systems in regulated fintech.
