# RELICK

> *Re-taste history. See what’s coming.*

---

RELICK is a market intelligence platform that connects historical world events to market movements using AI. It answers the question no financial platform answers well today: **why did markets move the way they did — and what does history suggest comes next?**

The platform operates on two layers: a free **Explorer** tier that annotates price charts with AI-generated event explanations (complete with a highlight-to-learn engine that explains any financial term in plain English), and a paid **Intelligence** tier that accepts any current headline or thesis, searches a structured and credibility-verified event knowledge graph for the closest historical parallels, and returns a professional analyst memo with asset class implications and confidence scores. The premium **Oracle** tier adds live parallel monitoring, proactive alerts, and a community consensus layer showing what serious investors are watching and where they diverge from historical baseline.

The business model is a three-tier SaaS subscription: Explorer (free), Analyst ($29/month), Oracle ($99/month). The moat is the event knowledge graph — a continuously compounding, verified, causally-linked, vector-embedded record of global financial history that grows more valuable and less replicable every day the platform operates.

**Target acquirers at exit:** Bloomberg (~$990M in data acquisitions), LSEG/Refinitiv ($27B Reuters deal), S&P Global ($550M Kensho), BlackRock (Aladdin). **Target investors at seed:** Point72 Ventures, Ribbit Capital, a16z fintech.

---

## Document Index

| Document | Description |
|---|---|
| [📊 MARKET_RESEARCH.md](docs/MARKET_RESEARCH.md) | Total addressable market ($7.5B, 15.2% CAGR), live competitor analysis with scraped pricing (Bloomberg, Koyfin, Hedgeye, TradingView, 42Macro, Macro Hive, Quill, Visible Alpha), audience sizing, whitespace analysis, and distribution channels |
| [📈 BUSINESS_PLAN.md](docs/BUSINESS_PLAN.md) | Full business plan: executive summary, problem/solution, all three tiers in detail, go-to-market playbook, 3-year financial projections, unit economics (LTV:CAC >20:1), team structure, and funding ask |
| [⚙️ TECHNICAL_BLUEPRINT.md](docs/TECHNICAL_BLUEPRINT.md) | Complete technical architecture: recommended full stack (Next.js, FastAPI, Postgres+pgvector), data source evaluation, vector DB comparison (pgvector vs Pinecone vs Weaviate vs Qdrant), 4-agent system design with implementation code, AI prompt architecture, phased build roadmap with costs, and infrastructure scaling plan |
| [🗄️ DATA_STRATEGY.md](docs/DATA_STRATEGY.md) | Historical event sourcing strategy, deep GDELT evaluation and implementation guide with Python code, 5-tier credibility scoring system design, complete event knowledge graph schema (all SQL tables), real-time monitoring pipeline, data partnership targets, and 5-year moat-building roadmap |
| [💰 FUNDRAISING_PRIMER.md](docs/FUNDRAISING_PRIMER.md) | Comparable raises (2023–2025), investor-by-investor analysis (Point72, Ribbit, a16z, Coatue, Bessemer), pitch narrative with full verbal script, pre-revenue valuation benchmarks, ranked traction metrics, fundraising timeline from founding to Series A, and 13-slide pitch deck outline |
| [💡 BRAINSTORM.md](docs/BRAINSTORM.md) | No-limits document: wild feature ideas (War Room Mode, Dead Reckoning Score, Voice-Mode Parallel), pivot strategies if core thesis needs adjusting, partnership plays (Robinhood, Real Vision, Bloomberg), adjacent markets (political intelligence, CFO tools, insurance), the 10-year vision, enterprise product lines, white-label opportunities, and full API monetization strategy |
| [🛡️ RISK_AND_MOAT.md](docs/RISK_AND_MOAT.md) | Honest risk registry with 14 identified risks, deep dives on critical risks (data quality, AI hallucination, Bloomberg competition, regulatory), defensibility analysis at each growth stage, moat-building timeline and milestones, and acquisition vs. IPO scenario analysis with acquirer logic |

---

## Project Status

| Milestone | Status |
|---|---|
| Blueprint documents complete | ✅ Complete (April 2026) |
| Founding team | 🔄 In formation |
| Waitlist landing page | 🟡 Not started |
| MVP data pipeline (FRED, GDELT, EDGAR) | 🟡 Not started |
| Event knowledge graph v1 (2,000 events) | 🟡 Not started |
| Closed beta | 🟡 Not started |
| Public launch | 🟡 Not started |
| Seed round | 🟡 Not started |

---

## Quick Reference

**The name:** RELICK — simultaneously a relic (ancient artifact that survived time), a re-lick (sensory memory trigger that pulls you back to an exact moment), and a verb (*"Did you RELICK the 2008 setup?"*).

**The animal:** The elephant. Never forgets. Communicates through infrasound others can’t detect. The matriarch leads because her memory is the herd’s most valuable asset.

**The tiers:** Explorer (free, 5 AI credits/month) → Analyst ($29/mo, Submit Intel, 50 credits/rollover) → Oracle ($99/mo, live monitoring, alerts, community consensus, unlimited).

**The tech:** Next.js + FastAPI + Postgres/pgvector + Celery/Redis + OpenAI GPT-4o. 4-agent background system (Ingestion, Verification, Enrichment, Quality). Dual-signal matching: quantitative (price, macro, yield curves) + qualitative (narrative tone, geopolitical stress type, sentiment archetype).

**The moat:** A structured, credibility-verified, causally-linked event knowledge graph. Every day it runs, it’s harder to replicate. Every user interaction trains it. Every intel submission becomes proprietary signal.

---

*Repository maintained by the RELICK founding team. For questions: [founder contact TBD]*
