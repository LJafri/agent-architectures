# Agent Architectures

**Fifteen SVG architecture diagrams of how agentic AI systems work in production**, from inference and the context window through the agent loop, MCP, RAG, memory, orchestration, guardrails, evals, and observability.

**Live:** [ljafri.github.io/agent-architectures](https://ljafri.github.io/agent-architectures/)

| View | What it is | Live | File |
|---|---|---|---|
| **Atlas** | All 15 figures, each with design notes and failure modes | [index.html](https://ljafri.github.io/agent-architectures/) | `index.html` |
| **Speedrun** | 15 one-idea flash cards with a check-off progress bar, about 5 minutes | [speedrun.html](https://ljafri.github.io/agent-architectures/speedrun.html) | `speedrun.html` |
| **One-glance flow** | The whole architecture as one traceable line, five beats, animated | [flow.html](https://ljafri.github.io/agent-architectures/flow.html) | `flow.html` |

## Why this exists

Most agent diagrams stop at the model and its tools. These cover the parts that decide whether a system holds up in production: how the context window is assembled and budgeted, where permissions and retries live, what a tool call looks like on the wire, how the loop terminates, and the eval and observability machinery behind it.

I built this as the reference I wanted when designing and reviewing agentic systems for a regulated fintech. Each figure is drawn at the level where architecture decisions get made and comes with two columns:

- **Design notes**: the decisions that matter and why.
- **What breaks**: the failure modes that bite in production, drawn from building and operating these systems rather than from tutorials.

The figures are kept current. When a spec or convention moves (the MCP 2026-07-28 stateless revision, the OpenTelemetry GenAI semantic conventions), the affected figure is redrawn and the change is logged below.

## The 15 figures

Titles match the `<h3>` of each figure in `index.html`; anchors are `#fig-01` through `#fig-15`.

| Part | Figure | Title |
|---|---|---|
| **I. The Substrate** | 01 | From prompt to token to inference |
| | 02 | The inference serving path |
| | 03 | Context window assembly |
| **II. The Agent Core** | 04 | The agent loop |
| | 05 | Anatomy of a tool call |
| | 06 | MCP: the integration bus |
| | 07 | The RAG pipeline, both lanes |
| | 08 | Memory architecture |
| **III. Scale & Safety** | 09 | Multi-agent orchestration |
| | 10 | Guardrails & the human-in-the-loop |
| | 11 | Failure modes → recovery, mapped |
| **IV. Evals & Observability** | 12 | The eval harness |
| | 13 | LLM-as-judge, calibrated |
| | 14 | The observability plane |
| | 15 | The data flywheel |

Figures cross-reference each other (FIG 05 validates what FIG 04 emits; FIG 14's traces become FIG 12's eval cases; FIG 15 closes the loop). Part IV is the section most teams build last and need first.

The speedrun compresses the same 15 figures into 15 cards, one per figure, in the same order. The one-glance flow collapses the four parts into five beats on a single line: tokens in, context assembly, the agent loop (tools and MCP, RAG, memory), safety, and the evals and observability rail underneath.

## Run it locally

Open any of the three HTML files directly in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/`. No build step, no dependencies.

## Hosting

The site is deployed to GitHub Pages by `.github/workflows/pages.yml`. On every push to `main` the workflow stages the three HTML files plus `.nojekyll` and publishes them with `actions/deploy-pages`. It can also be run by hand from the Actions tab.

One-time repository setting, needed before the first deploy: **Settings → Pages → Build and deployment → Source: GitHub Actions**. Until that is set, the workflow's deploy step fails with a "Pages not enabled" error.

The `.nojekyll` file tells Pages to serve the files as-is instead of running them through Jekyll.

## How it's built

- One self-contained HTML file per view: inline CSS, inline SVG, a few dozen lines of vanilla JS (scrollspy, reveal-on-scroll, animation, progress state in `localStorage`). No frameworks, no chart libraries.
- The only external requests are Google Fonts (Inter and JetBrains Mono).
- A shared SVG visual language: teal = data path, violet = control/loop, amber = eval & feedback signal, red = failure/blocked, dashed = async/optional.
- Design tokens match [ljafri.me](https://ljafri.me) (slate dark surfaces, Inter + JetBrains Mono). The header, footer and "get in touch" links point back to ljafri.me.

## Revision log

- **Nov 2026**: accuracy pass against upstream sources. FIG 02 now states that rate limits apply to requests per minute as well as input and output tokens per minute, with cache reads exempt from the input-token limit on the Claude API. FIG 06 names `subscriptions/listen` as the replacement for `resources/subscribe` and the GET stream, and gives the exact responses a 2026-07-28-only server returns to legacy clients (405 for GET/DELETE, 404 plus `Method not found` for `initialize`). FIG 08 separates server-side compaction from context editing and describes Agent Skills as an open standard. FIG 14 records that the GenAI semantic conventions moved to their own repository in mid-2026 and shipped v0.1.0 in November 2026. Speedrun card 02 describes prefill and decode by their hardware bound rather than as fast and slow. Sidebar navigation labels now match the figure and part headings exactly.
- **Nov 2026**: hosted on GitHub Pages via a deploy workflow; canonical and Open Graph URLs point at the Pages site; the atlas's portfolio-relative links and favicon replaced with absolute links and an inline SVG icon so the pages work standalone. README figure titles aligned to the figures as drawn.
- **Nov 2026**: FIG 06 redrawn for MCP revision 2026-07-28 (stateless protocol: no `initialize` handshake, no `Mcp-Session-Id`, version and capabilities in `_meta`, `server/discover`, multi-round-trip requests, `Mcp-Method`/`Mcp-Name` headers; roots, sampling, logging deprecated; `resources/subscribe` removed). FIG 14 span names aligned to the OpenTelemetry GenAI semantic conventions (`invoke_agent`, `chat`, `execute_tool`, `gen_ai.evaluation.result`). FIG 05 adds programmatic tool calling; FIG 08 adds provider memory tools, server-side compaction, and skills as the procedural store.
- **Jul 2026**: initial 15 figures, speedrun and one-glance flow views. Split out of a personal monorepo into this repository.

## Reuse

Figures and text © Laraib Jafri. Reuse with attribution and a link back is welcome; open an issue for anything beyond that.

## Author

Laraib Jafri · [ljafri.me](https://ljafri.me) · architect and engineer building agentic systems in regulated fintech.
