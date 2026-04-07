# RELICK — Technical Blueprint

> **Version:** 1.0 | **Date:** April 2026  
> **Audience:** Engineering leads, CTOs, technical investors  
> **Status:** Pre-build architecture design

---

## Table of Contents
1. [System Architecture Overview](#1-system-architecture-overview)
2. [Recommended Full Stack](#2-recommended-full-stack)
3. [Data Source Evaluation](#3-data-source-evaluation)
4. [Vector Database Comparison](#4-vector-database-comparison)
5. [Agent Architecture Design](#5-agent-architecture-design)
6. [AI Pipeline & Prompt Architecture](#6-ai-pipeline--prompt-architecture)
7. [Phased Build Roadmap](#7-phased-build-roadmap)
8. [Infrastructure Scaling Plan](#8-infrastructure-scaling-plan)

---

## 1. System Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│ RELICK PLATFORM │
├─────────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ │
│ │ FRONTEND │ │ API GATEWAY │ │ AGENT SYSTEM │ │
│ │ Next.js 14 │◄───►│ FastAPI │◄───►│ (4 agents) │ │
│ │ React │ │ Python │ │ Always-on │ │
│ └─────────────────┘ └─────────────────┘ └─────────────────┘ │
│ │
│ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ │
│ │ POSTGRES + │ │ REDIS CACHE │ │ OBJECT │ │
│ │ pgvector │ │ + QUEUE │ │ STORAGE │ │
│ │ (event KB) │ │ (Celery) │ │ (S3/GCS) │ │
│ └─────────────────┘ └─────────────────┘ └─────────────────┘ │
│ │
│ ┌─────────────────┐ ┌─────────────────┐ │
│ │ AI LAYER │ │ EXTERNAL DATA │ │
│ │ OpenAI GPT-4o │ │ FRED, GDELT, │ │
│ │ Embeddings │ │ EDGAR, Yahoo │ │
│ └─────────────────┘ └─────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Recommended Full Stack

### Frontend

| Layer | Technology | Rationale |
|---|---|---|
| Framework | **Next.js 14 (App Router)** | SSR for SEO on public chart pages; RSC for performance; strong ecosystem |
| UI Library | **React 18** | Component model maps well to chart+annotation architecture |
| Charting | **Lightweight Charts (TradingView lib)** | Free, performant, identical feel to TradingView; handles OHLCV natively |
| State | **Zustand** | Lightweight; avoids Redux overhead for MVP |
| Styling | **Tailwind CSS + shadcn/ui** | Fast iteration, clean design system, dark mode built-in |
| Auth | **Clerk** | Fastest path to auth with JWT, social login, and subscription hooks |
| Payments | **Stripe** | Industry standard; subscription management built-in; handles trials/upgrades |
| Analytics | **PostHog** | Self-hostable; captures highlight events, credit usage, conversion funnel |
| Notifications | **Novu** | Open-source notification infrastructure; handles email + push + in-app |

### Backend / API

| Layer | Technology | Rationale |
|---|---|---|
| API Framework | **FastAPI (Python)** | Native async; ideal for AI workloads; automatic OpenAPI docs; great DX |
| Task Queue | **Celery + Redis** | Background jobs for agent system; event ingestion; parallel matching jobs |
| Message Broker | **Redis** | Serves dual role: cache + Celery broker. Simple ops at seed stage. |
| Authentication | **JWT via Clerk webhook** | Backend validates Clerk JWTs; no custom auth complexity |
| API Rate Limiting | **FastAPI-Limiter (Redis-backed)** | Enforces credit system; protects against abuse |

### Data Layer

| Layer | Technology | Rationale |
|---|---|---|
| Primary DB | **PostgreSQL 16** | Relational integrity for events, users, sources; ACID compliance |
| Vector Search | **pgvector extension** | Avoids separate vector DB at MVP; vectors live alongside structured data |
| Cache | **Redis** | Chart data, frequently-accessed events, session data |
| Object Storage | **AWS S3 / Cloudflare R2** | Store pre-rendered chart data, audio (future), PDFs |
| Search | **PostgreSQL full-text search** | Sufficient at MVP; upgrade to Elasticsearch if needed at scale |
| Time Series | **TimescaleDB extension** | Market price data is time-series; Timescale adds hypertables to Postgres |

### Infrastructure

| Layer | Technology | Rationale |
|---|---|---|
| Cloud | **AWS (primary)** | Mature ecosystem; RDS for Postgres; ElastiCache for Redis; great DX |
| Container | **Docker + Docker Compose** | Simple dev/prod parity at MVP |
| Orchestration | **AWS ECS (Fargate)** | Serverless containers; no K8s overhead until 50K+ users |
| CI/CD | **GitHub Actions** | Native to repo; free at seed scale |
| CDN | **Cloudflare** | Edge caching for chart data + DDoS protection |
| Monitoring | **Datadog** | APM, logs, infra metrics in one; worth the cost for debugging AI pipeline |
| Secrets | **AWS Secrets Manager** | Rotate API keys without deploys |

---

## 3. Data Source Evaluation

### Source Tier Assessment

| Source | Data Type | Coverage | Cost | Quality | Priority |
|---|---|---|---|---|---|
| **FRED API** | Macro (GDP, inflation, rates, unemployment) | 1950s–present | **Free** | Excellent | P0 |
| **GDELT Project** | Global news events | 1979–present | **Free** | Good (raw) | P0 |
| **SEC EDGAR** | Filings, earnings, insider transactions | 1993–present | **Free** | Excellent | P0 |
| **Yahoo Finance (yfinance)** | Price/OHLCV data | 1970s–present | **Free** | Good | P0 |
| **World Bank Open Data** | Global macro, development indicators | 1960s–present | **Free** | Excellent | P1 |
| **Wikipedia API** | Landmark event context, historical background | Full history | **Free** | Good | P1 |
| **NewsAPI** | Real-time news | Live + 1mo history | $449–$2,999/mo | Good | P1 |
| **Federal Reserve (Beige Book, transcripts)** | Central bank language/tone | 1996–present | **Free** | Excellent (Tier 1) | P0 |
| **Congressional Record API** | Legislative context | 2000s–present | **Free** | Excellent (Tier 1) | P1 |
| **BLS (Bureau of Labor Statistics)** | Employment, CPI data | 1940s–present | **Free** | Excellent | P1 |
| **NBER Working Papers** | Academic macro research | 1970s–present | **Free** | Excellent (Tier 3) | P1 |
| **Alpha Vantage** | Financial data, fundamentals | 20+ years | Free tier / $50–$500/mo | Good | P2 |
| **Polygon.io** | Market data, tick-level | Full history | $29–$999/mo | Excellent | P2 |
| **Refinitiv / LSEG** | Comprehensive financial data | Full history | $5K–$50K/mo | Excellent | P3 (partnership) |

### MVP Data Stack (Month 1–6: $0 External Data Cost)

```
FRED API → Macro indicators (unemployment, CPI, Fed Funds, yield curve)
GDELT → Global event stream (raw events, needs enrichment pipeline)
SEC EDGAR → Earnings, filings, 8-Ks for market-moving corporate events  
Yahoo Finance (yfinance) → Price/OHLCV for all chart rendering
Fed Reserve website → FOMC transcripts, Beige Book (scrape/parse)
Wikipedia API → Contextual background for landmark events
```

**MVP achieves:** Full chart rendering, macro data from 1950s, global event context from 1979, corporate event annotations from 1993, real-time price data. This is a fully functional product with $0 in external data costs.

### Data Source Risks

| Risk | Source | Mitigation |
|---|---|---|
| API rate limits | FRED, EDGAR | Cache aggressively; pre-fetch and store normalized data |
| GDELT data quality | GDELT | Verification pipeline; never use GDELT event directly without enrichment |
| Yahoo Finance ToS | Yahoo Finance | Switch to Alpha Vantage or Polygon if ToS becomes issue; budget $50/mo |
| Wikipedia content quality | Wikipedia | Use as supplementary context only; verify key claims against Tier 1 sources |
| NewsAPI cost at scale | NewsAPI | Budget $449–$2,999/mo at Year 1 scale; explore GDELT as partial substitute |

---

## 4. Vector Database Comparison

### Decision Matrix

| Criteria | **pgvector** | **Pinecone** | **Weaviate** | **Qdrant** |
|---|---|---|---|---|
| **Setup complexity** | Low (Postgres extension) | Very low (managed SaaS) | Medium (Docker or cloud) | Low (Docker or cloud) |
| **Operational overhead** | Low (same DB) | Minimal (fully managed) | Medium | Low |
| **Performance (1M vectors)** | Good | Excellent | Excellent | Excellent |
| **Performance (10M+ vectors)** | Needs tuning (HNSW) | Excellent | Excellent | Excellent |
| **Hybrid search (keyword + vector)** | Manual implementation | Limited | Native | Native |
| **Filtering** | SQL-native | Good | GraphQL | Native |
| **Cost (10K vectors)** | ~$0 (infra) | ~$0 (free tier) | ~$0 (self-hosted) | ~$0 (self-hosted) |
| **Cost (1M vectors)** | ~$50–$200/mo (RDS) | ~$140/mo (Starter) | ~$50–$300/mo | ~$50–$200/mo |
| **Cost (10M vectors)** | ~$300–$800/mo | ~$1,400–$3,000/mo | ~$300–$1,000/mo | ~$300–$800/mo |
| **Ecosystem maturity** | Excellent (Postgres) | Excellent | Good | Good |
| **Multi-tenancy** | Yes (schema-level) | Namespaces | Multi-tenancy support | Collections |
| **Best for** | MVP, <5M vectors | Production scale, SaaS | Advanced semantic search | High performance, open-source |

### Recommendation: pgvector → Qdrant Migration Path

**MVP (0–12 months):** Use **pgvector**  
Rationale: Zero additional infrastructure. Vectors live in the same Postgres instance as structured event data. SQL joins between event metadata and vector similarity search are natural and efficient. HNSW indexing in pgvector handles up to ~5M vectors well. This eliminates the operational overhead of maintaining a separate vector database when the team is 2–3 people.

**Scale (12–24 months, if event graph > 5M vectors):** Migrate to **Qdrant**  
Rationale: Qdrant is the best open-source option for self-hosted production use. Rust-based — exceptional performance. Native filtering. No per-vector pricing cliff (unlike Pinecone). Payload filtering allows combining vector similarity with structured metadata (event date, credibility tier, asset class) without secondary lookups.

**Why not Pinecone:** Per-vector pricing becomes expensive at scale. The RELICK event knowledge graph is designed to grow to millions of events, making Pinecone's pricing model misaligned.

**Why not Weaviate:** Higher operational complexity for marginal benefit over Qdrant at RELICK's use case.

---

## 5. Agent Architecture Design

The RELICK agent system runs as a **continuous background process** independent of the user-facing API. Four specialized agents handle data lifecycle.

### Agent 1: Ingestion Agent

**Purpose:** Monitor live feeds and classify new events into the knowledge graph staging area

**Architecture:**
```python
class IngestionAgent:
    # Runs every 15 minutes via Celery beat
    
    sources = [
        GDELTStream(),       # Global news event stream
        EDGARFiler(),        # SEC EDGAR new filings (8-K, 10-K, DEF 14A)
        FREDUpdater(),       # Macro indicator updates
        FedRSSParser(),      # Federal Reserve press releases, speeches
        NewsAPIClient(),     # Real-time news monitoring
    ]
    
    async def run(self):
        for source in self.sources:
            raw_events = await source.fetch_new()
            classified = await self.classify(raw_events)  # LLM classification
            await self.stage(classified)  # Write to staging table
```

**LLM Classification Task:**
For each raw event, the Ingestion Agent uses GPT-4o-mini (cost: ~$0.001/event) to:
1. Determine event type (monetary policy, geopolitical, corporate, macro, natural disaster, regulatory)
2. Extract affected asset classes (equities, bonds, FX, commodities, crypto)
3. Assign preliminary severity score (1–10)
4. Extract named entities (companies, countries, institutions, people)
5. Generate initial embedding (text-embedding-3-small, 1536 dims)

---

### Agent 2: Verification Agent

**Purpose:** Cross-reference staged events against 3+ independent sources before promotion to knowledge graph

**Architecture:**
```python
class VerificationAgent:
    # Runs every 2 hours via Celery beat
    MINIMUM_SOURCES = 2
    MINIMUM_TIER = 3  # Tier 3 or better (no unverified sources)
    
    async def verify(self, staged_event: StagedEvent) -> VerifiedEvent | None:
        sources_found = await self.search_corroborating_sources(staged_event)
        
        tier1_count = len([s for s in sources_found if s.credibility_tier <= 2])
        tier2_count = len([s for s in sources_found if s.credibility_tier <= 3])
        
        if tier2_count < self.MINIMUM_SOURCES:
            return None  # Event rejected; never enters knowledge graph
        
        credibility_score = self.calculate_credibility_score(sources_found)
        return VerifiedEvent(event=staged_event, sources=sources_found, score=credibility_score)
```

**Credibility Tier System:**

| Tier | Score | Sources | Notes |
|---|---|---|---|
| Tier 1 | 1.0 | Fed transcripts, SEC filings, Congressional records, IMF/World Bank releases | Primary source; highest weight |
| Tier 2 | 0.85 | WSJ, FT, Reuters, Bloomberg, AP, BBC | Established financial press |
| Tier 3 | 0.80 | NBER papers, peer-reviewed academic journals | Research validation |
| Tier 4 | 0.65 | Economist, NYT, The Guardian, Le Monde | Quality general press |
| Tier 5 | 0.0 | Unverified, social media, anonymous sources | **Never enters knowledge graph** |

**Composite Score Formula:**
```
credibility_score = (
    max_tier_score × 0.5 +          # Best source tier (50% weight)
    source_count_bonus × 0.3 +      # Number of independent sources (30% weight)
    cross_source_agreement × 0.2    # LLM-assessed factual agreement between sources (20% weight)
)
```

---

### Agent 3: Enrichment Agent

**Purpose:** Deepen verified events with causal context, linked events, and market reaction data

**Tasks:**
1. **Causal chain extraction:** "What caused this event?" and "What did this event cause?" — links events in a directed graph
2. **Market reaction tagging:** Pulls OHLCV data for relevant assets on the event date +/- 5 days; computes standardized returns
3. **Regime context:** Tags the macro regime at event time (yield curve shape, Fed stance, credit spread level)
4. **Related event linking:** Vector similarity search to find related events in the existing graph; adds edges
5. **Asset class impact scoring:** Rates 1–10 impact on equities, bonds, FX, commodities for the event type
6. **Full embedding generation:** Generates high-quality embedding using text-embedding-3-large (3072 dims) for the complete enriched event

**Output schema per event:**
```json
{
  "event_id": "uuid",
  "title": "string",
  "date": "ISO 8601",
  "event_type": "monetary_policy | geopolitical | corporate | macro | regulatory | crisis",
  "summary": "string (200-400 words, LLM-generated)",
  "causal_predecessors": ["event_id"],
  "causal_successors": ["event_id"],
  "affected_assets": {
    "equities": {"impact_score": 8, "direction": "negative", "sectors": ["financials", "real_estate"]},
    "bonds": {"impact_score": 7, "direction": "positive"},
    "fx": {"impact_score": 5, "direction": "usd_positive"},
    "commodities": {"impact_score": 3, "direction": "neutral"}
  },
  "macro_regime_at_event": {
    "yield_curve_slope": "inverted",
    "fed_stance": "tightening",
    "sp500_trailing_return_12m": -18.2,
    "credit_spread_level": "elevated"
  },
  "market_reactions": {
    "sp500_1d": -3.2, "sp500_5d": -8.1, "sp500_30d": -15.4,
    "us10y_yield_1d": 0.15, "usd_index_1d": 0.8
  },
  "credibility_score": 0.92,
  "sources": [
    {"url": "...", "publisher": "WSJ", "tier": 2, "title": "..."}
  ],
  "embedding_small": [/* 1536 dims */],
  "embedding_large": [/* 3072 dims */],
  "tags": ["banking_crisis", "fed_response", "contagion"],
  "geographic_scope": ["US", "EU"],
  "created_at": "ISO 8601",
  "last_enriched": "ISO 8601"
}
```

---

### Agent 4: Quality Agent

**Purpose:** Nightly maintenance of knowledge graph integrity

**Tasks (runs nightly at 2am UTC):**
1. **Staleness audit:** Flag events with stale information (cited URLs returning 404, retracted articles)
2. **Consistency check:** Verify that causal edges are bidirectional and coherent
3. **Embedding refresh:** Re-embed events whose summaries have been updated
4. **Duplicate detection:** Find near-duplicate events (cosine similarity > 0.97); merge or flag for human review
5. **Coverage gap detection:** Identify dates with major market moves but no annotated events; prioritize for enrichment
6. **Score recalibration:** Recalculate credibility scores if source tier assignments have changed

---

## 6. AI Pipeline & Prompt Architecture

### Core AI Workflows

#### Workflow 1: Chart Annotation Generation
**Trigger:** User views a chart segment with no existing annotation  
**Model:** GPT-4o (quality matters here; this is user-facing)  
**Latency target:** < 3 seconds (streaming response preferred)

```python
SYSTEM_PROMPT = """
You are RELICK's market historian. You explain WHY markets moved, not just that they moved.
Your explanations connect specific world events to specific market reactions.
You write with authority but in plain English that a serious retail investor can understand.
You always cite the specific event that triggered the move, the mechanism by which it affected markets,
and what happened next. You are not a financial advisor — you explain history, not the future.

CRITICAL RULES:
- Never speculate about future markets
- Only cite verified events from the RELICK knowledge graph
- Include the credibility score of the underlying events
- Explain mechanisms (WHY bonds rise when stocks fall, not just that they did)
- Maximum 150 words per annotation
"""

USER_PROMPT = """
Chart context: {asset} price chart from {start_date} to {end_date}
Relevant events in knowledge graph: {json.dumps(events)}
Price action: {price_change}% over period, notable inflections at: {inflection_points}

Generate an annotation explaining the dominant market narrative for this period.
"""
```

#### Workflow 2: Submit Intel — Parallel Matching
**Trigger:** Analyst/Oracle user submits a headline or thesis  
**Model:** GPT-4o for signal extraction + text-embedding-3-large for similarity search  
**Latency target:** < 15 seconds (complex operation; show progress UI)

**Step 1: Signal Extraction**
```python
EXTRACTION_PROMPT = """
Extract structured signals from this input: {user_input}

Return JSON with:
- quantitative_signals: {{yield_curve_shape, credit_spread_level, equity_momentum, volatility_level}}
- qualitative_signals: {{central_bank_tone, geopolitical_stress_type, fiscal_posture, market_sentiment_archetype}}
- primary_asset_classes_affected: list
- temporal_horizon: short_term | medium_term | long_term
- thesis_summary: string (50 words max)
"""
```

**Step 2: Dual-Signal Matching**
```python
# Quantitative match: structured metadata filtering
quant_candidates = db.query("""
    SELECT event_id, 
           (1 - (embedding_large <=> $1)) AS vector_similarity,
           ABS(regime_yield_slope - $2) AS slope_diff
    FROM events
    WHERE credibility_score >= 0.7
    ORDER BY embedding_large <=> $1
    LIMIT 50
""", [query_embedding, user_yield_slope])

# Qualitative match: re-rank by qualitative signal alignment
reranked = llm_rerank(quant_candidates, qualitative_signals)
top_3 = reranked[:3]
```

**Step 3: Analyst Memo Generation**
```python
MEMO_PROMPT = """
You are a senior portfolio strategist at a tier-1 macro hedge fund.
A client has submitted this thesis: {thesis_summary}

You have identified these 3 historical parallels (ranked by confidence):
{parallel_1}: {summary} | Confidence: {score} | Date: {date}
{parallel_2}: {summary} | Confidence: {score} | Date: {date}  
{parallel_3}: {summary} | Confidence: {score} | Date: {date}

Write a professional analyst memo (350-500 words) covering:
1. Why these parallels are relevant (mechanisms, not just correlation)
2. How markets responded in each parallel across: equities, bonds, FX, commodities
3. What's different this time (regime differences, structural changes)
4. Asset class implications — not recommendations, implications
5. Key variables to watch that would confirm or invalidate the parallel

Tone: authoritative, precise, intellectually honest about uncertainty.
Do NOT give investment advice. Frame as historical context only.
"""
```

#### Workflow 3: Highlight-to-Learn Engine
**Trigger:** User highlights any text in any annotation  
**Model:** GPT-4o-mini (cost-optimized; simpler task)  
**Latency target:** < 1.5 seconds

```python
HIGHLIGHT_PROMPT = """
The user highlighted: "{selected_text}" in the context of: "{surrounding_context}"

Explain this term/concept in 2-3 sentences as if talking to a smart person who is not a finance professional.
Focus on: what it is, why it matters to markets, and give one concrete historical example.
Do not use jargon without explaining it. Do not exceed 80 words.
"""
```

---

## 7. Phased Build Roadmap

### Phase 0: Foundation (Months 1–2)
**Goal:** Infrastructure up, data pipeline running, internal demo  
**Team:** 2 engineers (full-stack + data)  
**Estimated Cost:** $15,000–25,000 (infra + API costs)

| Deliverable | Details |
|---|---|
| Postgres + pgvector setup | RDS instance, schema design, migration system |
| FRED data pipeline | Ingest 50+ macro series back to 1950s; normalize to standard schema |
| Yahoo Finance price data | Historical OHLCV for S&P 500 components + major indices |
| GDELT initial ingest | 2010–2024 global event stream; basic classification |
| Basic chart renderer | Next.js + Lightweight Charts; plot OHLCV + macro overlays |
| Ingestion Agent v1 | Scheduled FRED + EDGAR pull; writes to staging table |

### Phase 1: MVP — Explorer Tier (Months 3–5)
**Goal:** Working free product; waitlist conversion begins  
**Team:** 2–3 engineers + 1 product  
**Estimated Cost:** $30,000–50,000

| Deliverable | Details |
|---|---|
| Event knowledge graph v1 | 2,000+ verified historical events; 1990–2024 |
| Chart annotation engine | AI-generated annotations triggered by chart navigation |
| Highlight-to-learn engine | Highlight any term → instant explanation |
| Verification Agent v1 | 2-source minimum check; credibility scoring |
| User auth + accounts | Clerk integration; free tier signup |
| Credit system | 5 AI credits/month for Explorer; usage tracking |
| Landing page | Waitlist conversion + product demo |

### Phase 2: Analyst Tier + Submit Intel (Months 6–8)
**Goal:** First paid tier live; revenue begins  
**Team:** 3–4 engineers + 1 product  
**Estimated Cost:** $50,000–80,000

| Deliverable | Details |
|---|---|
| Submit Intel workflow | Full input → parallel matching → memo output pipeline |
| Dual-signal matching engine | Quant + qualitative parallel search |
| Confidence scoring system | Composite score for each historical parallel |
| Stripe integration | Analyst tier billing; trials; upgrades |
| Credit rollover system | 50 credits/month with 150 cap |
| Enrichment Agent v1 | Causal chain extraction; market reaction tagging |
| Event graph: 10,000+ events | Full enrichment pipeline running |

### Phase 3: Oracle Tier + Live Monitoring (Months 9–12)
**Goal:** Full product live; hit $50K MRR  
**Team:** 4–5 engineers + 1 product + 1 growth  
**Estimated Cost:** $80,000–$120,000

| Deliverable | Details |
|---|---|
| Live parallel monitoring | Continuous matching against live market conditions |
| Alert system | Push notifications via Novu; parallel match alerts |
| Forward parallel matching | Probabilistic scenario output generation |
| Community intel layer | User submissions; consensus divergence scoring |
| Oracle billing + features | Full Oracle tier live |
| Quality Agent v1 | Nightly graph maintenance |
| Mobile-responsive | Full responsive UI; push notification support |

### Phase 4: Scale + Enterprise Foundation (Months 13–18)
**Goal:** $100K MRR; Series A readiness  
**Team:** 6–8 engineers + full leadership  
**Estimated Cost:** $150,000–$250,000

| Deliverable | Details |
|---|---|
| API v1 (beta) | Programmatic access to knowledge graph for Oracle users |
| Enterprise tier foundations | Team accounts, custom data, white-label options |
| pgvector → Qdrant migration | If event graph > 5M vectors |
| Historical depth expansion | Pre-1979 events via manual curation + NBER papers |
| Data partnerships | First paid data licensing conversation |
| Performance optimization | Sub-100ms chart load; sub-3s annotation generation |

---

## 8. Infrastructure Scaling Plan

### Stage 1: MVP (0–1,000 users)

| Service | Configuration | Monthly Cost |
|---|---|---|
| RDS Postgres (pgvector) | db.t3.medium, 100GB | ~$60 |
| ElastiCache Redis | cache.t3.micro | ~$20 |
| ECS Fargate (API) | 1 vCPU, 2GB, 2 tasks | ~$50 |
| ECS Fargate (Agents) | 0.5 vCPU, 1GB, 2 tasks | ~$25 |
| S3 (object storage) | 100GB | ~$5 |
| CloudFront CDN | 1TB transfer | ~$85 |
| **Total Infra** | | **~$245/mo** |

### Stage 2: Growth (1,000–10,000 users)

| Service | Configuration | Monthly Cost |
|---|---|---|
| RDS Postgres | db.r6g.large, 500GB, Multi-AZ | ~$350 |
| ElastiCache Redis | cache.r6g.large cluster | ~$180 |
| ECS Fargate (API) | 2 vCPU, 4GB, 4–8 tasks | ~$200 |
| ECS Fargate (Agents) | Scaled up | ~$150 |
| S3 + CloudFront | 5TB | ~$400 |
| Datadog | APM + infra | ~$200 |
| **Total Infra** | | **~$1,480/mo** |

### Stage 3: Scale (10,000–100,000 users)

| Service | Configuration | Monthly Cost |
|---|---|---|
| RDS Aurora (Postgres) | r6g.2xlarge cluster, 2TB | ~$1,200 |
| Qdrant Cloud (vector) | Production tier | ~$500 |
| ElastiCache Redis | cluster mode | ~$600 |
| ECS (API, scaled) | Auto-scaling, multiple tasks | ~$800 |
| Agent infrastructure | Dedicated instances | ~$500 |
| S3 + CloudFront | 20TB | ~$1,500 |
| Datadog | Full suite | ~$500 |
| **Total Infra** | | **~$5,600/mo** |

### AI API Cost Projections

| Stage | GPT-4o calls/day | GPT-4o-mini calls/day | Embedding calls/day | Monthly AI Cost |
|---|---|---|---|---|
| MVP (1K users) | 500 | 2,000 | 5,000 | ~$150 |
| Growth (10K users) | 5,000 | 20,000 | 50,000 | ~$1,500 |
| Scale (100K users) | 50,000 | 200,000 | 500,000 | ~$15,000 |

*AI costs are the primary variable cost driver. At scale, negotiating enterprise pricing with OpenAI (50% discount typical) or running open models (Llama 3.1 70B for lower-stakes tasks) can reduce this by 40–60%.*

### Performance Targets by Stage

| Metric | MVP Target | Growth Target | Scale Target |
|---|---|---|---|
| Chart load time | < 2s | < 1s | < 500ms |
| Annotation generation | < 5s | < 3s | < 2s |
| Submit Intel memo | < 30s | < 15s | < 10s |
| API uptime | 99% | 99.5% | 99.9% |
| Event graph size | 2,000 events | 25,000 events | 250,000 events |
