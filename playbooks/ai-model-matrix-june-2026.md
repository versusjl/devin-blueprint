# AI Model & Platform Matrix — June 30, 2026

> Benchmark figures are compiled from vendor announcements and public
> leaderboards as of the date above, not independently verified. Treat them as
> directional, verify before relying on any single number, and re-check
> quarterly — the model landscape moves fast.

## Your Subscriptions

| Platform | What It Is | Current Flagship Model(s) |
|----------|-----------|--------------------------|
| **OpenAI** | Closed-source API + ChatGPT | GPT-5.5 (flagship), GPT-5.5 Thinking (reasoning), GPT-5.6 Sol (preview, limited) |
| **Claude** | Closed-source API + claude.ai | Claude Opus 4.8 (flagship), Sonnet 4.6 (workhorse), Haiku 4.5 (fast/cheap) |
| **GLM** | Open-weight API (Zhipu AI / Z.ai) | GLM-5.2 (744B MoE, 40B active, MIT license) |
| **KIMI** | Open-weight API (Moonshot AI) | Kimi K2.7-Code (coding), K2.6 (general, multimodal) |
| **Devin Lite** | IDE + local agent (Devin Desktop/CLI, formerly Windsurf) | SWE-1.6 (local), routes to Claude/OpenAI/Gemini for complex tasks |
| **Devin** | Autonomous cloud agent (Devin Cloud) | Proprietary model + frontier model routing, full VM sandbox |

---

## Task-to-Model Recommendation Matrix

| Task Category | Best Choice | Runner-Up | Notes |
|--------------|-------------|-----------|-------|
| **Complex autonomous coding** (multi-file PRs, migrations, full features) | **Devin Cloud** | Claude Opus 4.8 | Devin Cloud operates in a full VM with shell, browser, git. Best for "fire and forget" tasks. 67% PR merge rate on migrations. |
| **Interactive coding** (pair programming, debugging, inline edits) | **Devin Lite (Desktop)** | Claude Sonnet 4.6 | IDE-native agent with full codebase context. Faster feedback loops than cloud. |
| **Hard bug fixing** (complex repo issues, SWE-bench-class) | **Claude Opus 4.8** | OpenAI GPT-5.5 | Opus 4.8: 88.6% SWE-bench Verified, 69.2% SWE-bench Pro. Best proven reliability. |
| **Terminal/CLI agentic tasks** | **OpenAI GPT-5.5** | GLM-5.2 | GPT-5.5: 82.7% Terminal-Bench 2.0. GLM-5.2: 81.0% Terminal-Bench 2.1. Both excellent. |
| **Long-horizon agentic coding** (sustained multi-step engineering) | **Claude Opus 4.8** | GLM-5.2 | Opus designed for deep orchestration; GLM-5.2 gaining fast (62.1% SWE-bench Pro). |
| **Tool use / MCP orchestration** | **Claude Opus 4.8** | OpenAI GPT-5.5 | Opus: 82.2% MCP Atlas. GPT-5.5: 75.3%. Opus has widest gap on multi-step tools. |
| **General writing** (emails, docs, marketing copy) | **Claude Sonnet 4.6** | OpenAI GPT-5.5 | Sonnet: high quality at low cost. GPT-5.5 slightly more polished but 2x price. |
| **Creative writing** (storytelling, marketing, persuasive copy) | **GLM-5.2** | Claude Sonnet 4.6 | GLM consistently produces more engaging hooks and creative angles. |
| **Research & deep analysis** | **OpenAI GPT-5.5** | Claude Opus 4.8 | GPT-5.5 excels at browsing + synthesis. Opus better at pure reasoning without tools. |
| **Math & competition reasoning** | **GLM-5.2** | OpenAI GPT-5.5 | GLM-5.2: 99.2% AIME 2026, 91.2% GPQA-Diamond. Near-ceiling on math benchmarks. |
| **Science Q&A / graduate-level reasoning** | **GLM-5.2** | OpenAI GPT-5.5 | GLM-5.2: 54.7% Humanity's Last Exam (w/ tools) vs GPT-5.5: 52.2%. |
| **Bilingual Chinese-English** | **GLM-5.2** | KIMI K2.6 | Both are Chinese-developed. GLM better for translation, KIMI stronger on coding. |
| **Budget agentic coding** (cost-sensitive, high volume) | **KIMI K2.7-Code** | GLM-5.2 | KIMI: $0.95/$4.00 per 1M tokens. GLM: ~$1.40/$4.40. Both 5-7x cheaper than frontier. |
| **Quick summarization / extraction** | **Claude Haiku 4.5** | Claude Sonnet 4.6 | Haiku: $1/$5 per 1M tokens, fastest latency. Perfect for high-volume simple tasks. |
| **Frontend/UI generation** | **GLM-5.2** | Claude Opus 4.8 | GLM-5.2: #1 on Code Arena: Frontend (1595 Elo). Strong at design-to-code. |
| **Code review** | **Devin Lite (Desktop)** | Claude Sonnet 4.6 | IDE context + inline suggestions. Devin Review also available as PR integration. |
| **Data analysis & spreadsheets** | **OpenAI GPT-5.5** | Claude Opus 4.8 | GPT-5.5 has superior computer-use + tool integration for data workflows. |
| **Customer-facing content** (support, responses) | **Claude Sonnet 4.6** | OpenAI GPT-5.5 | Sonnet: excellent quality/cost ratio for high-volume customer work. |
| **Self-hosted / on-prem needs** | **GLM-5.2** | KIMI K2.6 | Both open-weight (MIT). Only options if you need to run models on your infrastructure. |

---

## Detailed Platform Profiles

### OpenAI (GPT-5.5)

| Attribute | Details |
|-----------|---------|
| **Flagship** | GPT-5.5 (Apr 23, 2026) |
| **Context** | ~1M tokens (API) |
| **Pricing** | $5 input / $30 output per 1M tokens |
| **Key strengths** | Terminal agents (82.7% Terminal-Bench), tool use, computer use, deep research, data analysis, polished UX |
| **Key weaknesses** | Most expensive per-token, closed source, occasional hallucinated APIs |
| **Best for you** | Research workflows, data analysis, browsing + implementation combos, terminal-heavy agentic work |
| **Upcoming** | GPT-5.6 Sol (preview, limited access) — adds sub-agents and "ultra" mode |

### Claude (Anthropic)

| Attribute | Opus 4.8 | Sonnet 4.6 | Haiku 4.5 |
|-----------|----------|------------|-----------|
| **Context** | 1M tokens | 1M tokens | 200K tokens |
| **Pricing** | $5/$25 per 1M | $3/$15 per 1M | $1/$5 per 1M |
| **SWE-bench Verified** | 88.6% | 79.6% | — |
| **Best for** | Hard bugs, deep orchestration, long-horizon agency | 80% of daily work (coding, writing, analysis) | High-volume extraction, tagging, summarization |
| **Key strength** | Highest proven coding reliability + MCP tool use | Best cost/performance ratio in the industry | Fastest, cheapest near-frontier model |
| **Knowledge cutoff** | Jan 2026 | Aug 2025 | Feb 2025 |

**Note:** Claude Fable 5 (the Mythos-class tier above Opus) launched in June 2026 and is generally available; Claude Mythos 5 shares the same underlying model and is limited to approved organizations. Not yet reflected in the matrix above — re-evaluate the Claude rows once benchmarks land.

### GLM (Zhipu AI / Z.ai)

| Attribute | Details |
|-----------|---------|
| **Flagship** | GLM-5.2 (June 2026), 744B params, 40B active (MoE) |
| **Context** | 1M tokens |
| **Pricing** | ~$1.40 input / $4.40 output per 1M tokens |
| **License** | MIT (open-weight, self-hostable) |
| **Key strengths** | SWE-bench Pro (62.1%), Terminal-Bench 2.1 (81.0%), math (99.2% AIME), frontend (#1 Code Arena), creative writing, Chinese bilingual, extremely cost-effective |
| **Key weaknesses** | Consumer UX is developer-focused (not polished like ChatGPT/Claude), English docs lag, no native Western enterprise integrations (Slack, Notion), generalist reasoning weaker than coding scores suggest |
| **Best for you** | Budget-friendly agentic coding, frontend generation, math/science reasoning, Chinese content, self-hosting scenarios |

### KIMI (Moonshot AI)

| Attribute | Details |
|-----------|---------|
| **Flagship** | Kimi K2.7-Code (June 12, 2026), 1T params, 32B active (MoE) |
| **Context** | 256K tokens |
| **Pricing** | $0.95 input / $4.00 output per 1M tokens |
| **License** | Modified MIT (open-weight) |
| **Key strengths** | Cheapest frontier-adjacent coding model (5-7x cheaper than Opus/GPT-5.5), multimodal (text + image + video input), MCP tool use (81.1% MCP Mark Verified), 30% fewer thinking tokens than K2.6 |
| **Key weaknesses** | No independent SWE-bench scores yet (all benchmarks are first-party), smaller context (256K vs 1M for others), early-stage ecosystem, practitioners report benchmarks don't always match real-world results |
| **Best for you** | Cost-sensitive agentic coding, volume API work, experimentation, tasks where you need a cheap "first pass" before escalating to a more reliable model |

### Devin Lite (Devin Desktop / CLI)

| Attribute | Details |
|-----------|---------|
| **What it is** | IDE agent (formerly Windsurf) + Rust CLI with local execution |
| **Model** | SWE-1.6 (Cognition's proprietary, free) + routes to frontier models (Claude, OpenAI, Gemini) on paid plans |
| **Pricing** | Free tier / Pro $20/mo / Max $200/mo |
| **Key strengths** | Real-time pair programming, full codebase awareness via LSP, inline edits, tab completions, local execution (fast), Spaces for multi-agent orchestration, no context-window worry (indexed repo) |
| **Key weaknesses** | Local agent less autonomous than cloud, limited to your machine's compute, dependent on frontier model routing for hardest tasks |
| **Best for you** | Day-to-day interactive coding, code review, quick fixes, debugging sessions where you want fast feedback loops |

### Devin (Devin Cloud)

| Attribute | Details |
|-----------|---------|
| **What it is** | Fully autonomous cloud agent with isolated VM (shell, browser, editor) |
| **Model** | Proprietary + frontier model routing |
| **Pricing** | Included in Pro ($20/mo) and above; usage-based ACUs |
| **Key strengths** | Complete autonomy (fire and forget), full development environment, integrates with GitHub/Slack/Linear/CI, can browse web, install tools, run tests, create PRs end-to-end, 90% of Cognition's own code ships via cloud agents |
| **Key weaknesses** | Higher latency (minutes per session), costs more for long-running tasks, sometimes needs course-correction on ambiguous specs |
| **Best for you** | Autonomous PRs, migrations, tech debt cleanup, CI fixes, tasks you want done while you sleep, anything requiring a full environment (installing dependencies, running servers, browser testing) |

---

## Decision Flowchart

```
Is this a SOFTWARE ENGINEERING task?
├── YES → Do you want to be hands-on (pair programming)?
│   ├── YES → Devin Lite (Desktop/CLI)
│   └── NO → Is it well-specified enough to "fire and forget"?
│       ├── YES → Devin Cloud
│       └── NO → How hard is the coding task?
│           ├── Very hard (multi-file, complex logic) → Claude Opus 4.8
│           ├── Medium (standard features, fixes) → Claude Sonnet 4.6
│           └── Cost-sensitive / high-volume → KIMI K2.7 or GLM-5.2
│
├── NO → Is this WRITING / CONTENT?
│   ├── Creative / marketing → GLM-5.2
│   ├── Professional / polished → Claude Sonnet 4.6
│   └── Quick / high-volume → Claude Haiku 4.5
│
├── NO → Is this RESEARCH / ANALYSIS?
│   ├── Needs web browsing + synthesis → OpenAI GPT-5.5
│   ├── Pure reasoning / science → GLM-5.2 or Claude Opus 4.8
│   └── Data + spreadsheets → OpenAI GPT-5.5
│
└── NO → Is this MATH / SCIENCE?
    ├── Competition math → GLM-5.2
    ├── Graduate-level science → GLM-5.2
    └── General reasoning → OpenAI GPT-5.5 or Claude Opus 4.8
```

---

## Cost Comparison (per 1M tokens)

| Platform | Input | Output | Relative Cost |
|----------|-------|--------|---------------|
| Claude Haiku 4.5 | $1.00 | $5.00 | Cheapest (closed) |
| KIMI K2.7-Code | $0.95 | $4.00 | Cheapest overall |
| GLM-5.2 | $1.40 | $4.40 | Budget frontier |
| Claude Sonnet 4.6 | $3.00 | $15.00 | Mid-tier |
| Claude Opus 4.8 | $5.00 | $25.00 | Premium |
| OpenAI GPT-5.5 | $5.00 | $30.00 | Most expensive |

*Devin Lite and Devin Cloud use usage-based pricing (ACUs) that bundles compute + model costs. Direct token comparison not applicable.*

---

## Key Benchmark Scorecard (June 2026)

| Benchmark | Claude Opus 4.8 | GPT-5.5 | GLM-5.2 | KIMI K2.7 |
|-----------|----------------|---------|---------|-----------|
| SWE-bench Verified | **88.6%** | 82.6% (3rd-party) | ~62% | No independent data |
| SWE-bench Pro | **69.2%** | 58.6% | 62.1% | No independent data |
| Terminal-Bench | 74.6% (v2.1) | **82.7%** (v2.0) | 81.0% (v2.1) | — |
| MCP Atlas | 77.8% | 75.3% | 77.0% | 76.0% |
| AIME 2026 | — | — | **99.2%** | — |
| GPQA-Diamond | — | — | **91.2%** | — |
| HLE (w/ tools) | — | 52.2% | **54.7%** | — |
| FrontierSWE | — | 72.6% | **74.4%** | — |

---

## Recommended Default Stack

| Workflow | Tool | Rationale |
|----------|------|-----------|
| Autonomous PRs, migrations, CI fixes | **Devin Cloud** | Full environment, fire-and-forget, integrates with your GitHub/Linear/Slack |
| Daily coding (features, debugging) | **Devin Lite (Desktop)** | Fast feedback, IDE-native, uses your local codebase context |
| Hard technical problems (when Devin gets stuck) | **Claude Opus 4.8** | Highest proven coding reliability, best for manual deep-dives |
| Writing specs, docs, marketing | **Claude Sonnet 4.6** | Best quality/cost ratio for prose |
| Research + web synthesis | **OpenAI GPT-5.5** | Best at browsing, tool use, and turning raw info into structured output |
| Budget batch processing / experimentation | **KIMI K2.7** or **GLM-5.2** | 5-7x cheaper, good enough for first-pass or non-critical tasks |
| Chinese/bilingual content | **GLM-5.2** | Native Chinese, strong translation |
| Math/science reasoning | **GLM-5.2** | Near-ceiling scores on competition math and graduate science |

---

*Document generated June 30, 2026. Model landscape changes rapidly — recommend quarterly review.*
