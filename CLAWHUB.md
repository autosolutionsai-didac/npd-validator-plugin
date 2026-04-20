# NPD Validator — ClawHub Submission Context

## What This Is

A multi-agent product validation pipeline that evaluates new product concepts and produces
evidence-based GO / CONDITIONAL GO / REVISIT / NO-GO launch recommendations. Orchestrates
8 specialized agents across 4 phases to score 15 dimensions of product viability.

## The 8-Agent Pipeline

```
Phase 1: Research Coordinator        → gathers market data, competitor profiles, category facts
Phase 2: 5 Independent Evaluators    → each scores 3 dimensions (1-10) with cited evidence:
          • Market Demand              (category size, demand signals, timing)
          • Competitive Intelligence   (white space, competitor weakness, barriers to entry)
          • Brand Fit                  (brand alignment, portfolio coherence, audience fit)
          • Commercial Viability       (pricing power, unit economics, channel fit)
          • Consumer & Trends          (social sentiment, trend momentum, cultural fit)
Phase 3: Devil's Advocate            → reads all 5 evaluations, builds strongest NO-GO case
Phase 4: Consensus Director          → 3-round deliberation → weighted composite → verdict
```

### Key Architecture Detail

In Claude Code/Cowork, each evaluator runs as a **true subagent with its own context window** —
zero anchoring bias between evaluators. In OpenClaw, the same pipeline runs sequentially in a
single context with "mental isolation" discipline (see `openclaw-install.sh`).

## What It Validates

- **Market fundamentals**: category size, growth, CAGR, timing, demand signals
- **Competitive dynamics**: white space, competitor weakness, barriers, patent signals
- **Brand alignment**: portfolio coherence, audience fit, cannibalization risk
- **Commercial reality**: pricing power, unit economics, channel fit, retailer calendars, supply chain
- **Consumer signals**: social sentiment, trend momentum, cultural fit, appropriation checks
- **Regulatory**: approval timelines, safety testing, claims substantiation, per-market divergence
- **Launch context**: seasonality, horizon, bundle vs line vs single SKU, beachhead strategy

## Scoring Framework

- **15 dimensions** scored 1-10 with confidence ratings (HIGH/MEDIUM/LOW)
- **Weighted composite**: Market Demand 25%, Competitive Intel 20%, Brand Fit 15%, Commercial Viability 25%, Consumer & Trends 15%
- **Verdict thresholds**: GO (7.5-10), CONDITIONAL GO (5.5-7.4), REVISIT (3.5-5.4), NO-GO (1.0-3.4)
- **Override rules**: Fatal Flaw, Confidence, Devil's Advocate overrides can downgrade one level
- **Research depth**: 25-40+ web searches per validation, minimum 150 words reasoning per dimension

## Component Inventory

| File | Purpose | Size |
|------|---------|------|
| `skills/validate-product/SKILL.md` | Main orchestrator — entry detection, brief population, pipeline execution, iteration support | ~14KB |
| `skills/npd-methodology/SKILL.md` | Scoring rubrics, dimension definitions, verdict thresholds, report templates | ~6KB |
| `agents/npd-research-coordinator.md` | Phase 1: 8-12 web searches, fact-only context brief | ~6KB |
| `agents/npd-market-demand.md` | Phase 2: category size, demand signals, timing | ~6KB |
| `agents/npd-competitive-intel.md` | Phase 2: white space, competitor weakness, barriers | ~5KB |
| `agents/npd-brand-fit.md` | Phase 2: brand alignment, portfolio coherence, audience fit | ~5KB |
| `agents/npd-commercial-viability.md` | Phase 2: pricing power, unit economics, channel fit | ~8KB |
| `agents/npd-consumer-trends.md` | Phase 2: social sentiment, trend momentum, cultural fit | ~6KB |
| `agents/npd-devils-advocate.md` | Phase 3: adversarial stress-test, kill shot rating | ~4KB |
| `agents/npd-consensus-director.md` | Phase 4: conflict detection, challenge/defend, final synthesis | ~16KB |
| `openclaw-install.sh` | OpenClaw deployment script — translates to SOUL/AGENTS/MEMORY workspace | ~9KB |
| `.claude-plugin/plugin.json` | Plugin manifest | <1KB |
| `README.md` | Documentation | ~4KB |

## Works For

- Beauty & skincare (validated extensively)
- CPG / consumer packaged goods
- Food & beverage
- Wellness & supplements
- SaaS & B2B products (with automatic B2B evaluator adaptation)
- Bundles, collections, and multi-SKU line launches
- Multi-market launches (US, EU, UK, Australia, Japan)

## Hardening History

Validated across **45 test scenarios** spanning **7 rounds of adversarial autoresearch evaluation**
(inspired by Karpathy's autoresearch methodology). The autoresearch eval sets and results
are in the marketplace repo at `autoresearch/`.

## OpenClaw Deployment

The included `openclaw-install.sh` handles the full translation:
- Creates SOUL.md (personality, boundaries, tone)
- Creates AGENTS.md (pipeline operating instructions with single-context discipline)
- Creates MEMORY.md (continuity structure)
- Copies all 8 agent prompts as readable skills
- Copies methodology and orchestrator
- Seeds initial memory
- Verifies all 13 workspace files
- Prints next-step instructions

## GitHub Source

`https://github.com/autosolutionsai-didac/npd-validator-marketplace`

## Suggested ClawHub Category

`research-analysis` or `product-management`

## Tags

`npd`, `product-validation`, `market-research`, `multi-agent`, `consumer-goods`, `beauty`,
`cpg`, `b2b`, `saas`, `launch-strategy`, `competitive-analysis`

## Author

AutoSolutions.ai — Didac Fernandez Girona (didac@autosolutions.ai)

## License

MIT
