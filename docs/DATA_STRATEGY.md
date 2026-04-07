# RELICK — Data Strategy

> **Version:** 1.0 | **Date:** April 2026  
> **Audience:** Technical founders, data engineers, potential data partners, technical investors

---

## Table of Contents
1. [Data Philosophy](#1-data-philosophy)
2. [Historical Event Data Sourcing](#2-historical-event-data-sourcing)
3. [GDELT Deep Evaluation & Implementation Guide](#3-gdelt-deep-evaluation--implementation-guide)
4. [Credibility Scoring System Design](#4-credibility-scoring-system-design)
5. [Event Knowledge Graph Schema](#5-event-knowledge-graph-schema)
6. [Real-Time Monitoring Pipeline](#6-real-time-monitoring-pipeline)
7. [Data Partnership Opportunities](#7-data-partnership-opportunities)
8. [5-Year Data Moat Building Strategy](#8-5-year-data-moat-building-strategy)
9. [Advisory Notes](#9-advisory-notes)

---

## 1. Data Philosophy

RELICK's data strategy is built on a single foundational belief:

> **The structured event knowledge graph is the product. Everything else — the charts, the AI memos, the alerts — is the interface to the graph.**

This shapes every data decision:

1. **Quality over quantity.** A knowledge graph with 5,000 deeply verified, richly annotated, causally linked events is worth more than 500,000 scraped news headlines with no credibility scoring.

2. **Structure is the moat.** Unstructured data is a commodity. ChatGPT can hallucinate a description of the 2008 financial crisis from training data. It cannot query a structured graph where the event object has: `credibility_score: 0.96`, `causal_predecessor: [Lehman_filing]`, `sp500_30d_return: -33.5`, `fed_response_days: 7`, `parallel_events: [1998_LTCM, 1987_BlackMonday]`. Structure is irreplaceable.

3. **Verification is trust.** Every piece of data that enters the RELICK knowledge graph carries provenance. Every claim traces to a source. Every source carries a credibility tier. Users and institutional partners need to trust the graph absolutely. That trust is built one verified event at a time.

4. **Compounding is the strategy.** The knowledge graph's value compounds. In Year 1, it has 10,000 events. In Year 3, it has 250,000. In Year 5, it has 2M+ events covering every material market event since 1900 with machine-readable structure. The distance between RELICK's graph and a competitor starting today grows every day the platform operates.

---

## 2. Historical Event Data Sourcing

### Phase 1: MVP Data Stack (Months 1–6)
*Goal: Full coverage 1990–2024. Zero external data cost.*

#### FRED API (Federal Reserve Economic Data)
- **Coverage:** 800,000+ economic data series, back to 1940s
- **Key series for RELICK:**
  - `DFF` — Federal Funds Rate (1954–present)
  - `GS10`, `GS2` — 10-Year and 2-Year Treasury yields
  - `T10Y2Y` — 10-Year minus 2-Year yield spread (yield curve)
  - `UNRATE` — US Unemployment Rate (1948–present)
  - `CPIAUCSL` — CPI All Urban Consumers (1947–present)
  - `BAMLH0A0HYM2` — US High Yield OAS (credit spreads)
  - `DCOILWTICO` — WTI Crude Oil Price
  - `SP500` — S&P 500 Index daily
  - `VIXCLS` — CBOE Volatility Index (VIX)
  - `DTWEXBGS` — US Dollar Index
- **Implementation:** Simple REST API; key: free with registration; rate limit: 1000 req/day (generous)
- **RELICK use:** Macro regime tagging for every event in the knowledge graph; chart overlays; quantitative signal extraction

#### SEC EDGAR Full-Text Search API
- **Coverage:** All SEC filings 1993–present: 10-K, 10-Q, 8-K, DEF 14A, S-1, etc.
- **Key RELICK uses:**
  - 8-K filings: Identifies material corporate events (earnings misses, mergers, regulatory actions, executive changes)
  - 10-K risk factor sections: Training data for event type classification
  - Earnings transcripts: Qualitative signal extraction (management tone, guidance language)
- **Implementation:** EDGAR full-text search API is free; `efts.sec.gov/hits.json` endpoint
- **Rate limit:** 10 req/second (reasonable)

#### Yahoo Finance via yfinance
- **Coverage:** Price/OHLCV data for 20,000+ global symbols, 1970s–present
- **Key RELICK uses:** Chart rendering for all assets; market reaction calculation for events (+/-1d, +/-5d, +/-30d returns)
- **Implementation:** `yfinance` Python library; Ticker.history() method
- **Risk:** ToS gray area for commercial use; mitigation: switch to Alpha Vantage ($50/mo) or Polygon.io ($29/mo) before launch
- **Polygon.io (recommended upgrade):** $29/mo for historical; $79/mo for real-time. Reliable API, clear commercial ToS, tick-level data available

#### Federal Reserve Public Resources
- **FOMC Meeting Transcripts:** Published with 5-year lag; all transcripts 1993–2020 available as PDF
- **Beige Book:** Published 8x/year; full text available via RSS feed and Federal Reserve website
- **Federal Reserve Speeches:** `federalreserve.gov/newsevents/` — all governor/chair speeches in HTML
- **Implementation:** PDF parsing via `pdfplumber` or `pymupdf`; HTML scraping; all free
- **RELICK credibility:** These are Tier 1 sources — the highest credibility content in the system

#### Wikipedia API
- **Coverage:** Comprehensive coverage of landmark financial events
- **Key RELICK uses:** Supplementary context for event summaries; not used as primary source (Tier 4 content at best)
- **Implementation:** `wikipedia-api` Python library
- **Note:** Wikipedia content must be validated against Tier 1-3 sources before any claim enters the knowledge graph

---

### Phase 2: Enhanced Coverage (Months 6–12)
*Goal: Global coverage, real-time monitoring, pre-1990 historical events*

#### NewsAPI
- **Coverage:** Real-time news + 1-month rolling archive; 80,000+ sources globally
- **Pricing:** Developer ($449/mo), Business ($2,999/mo)
- **RELICK use:** Real-time event detection; primary feed for live monitoring pipeline
- **Implementation:** `/v2/everything` endpoint with keyword filtering for financial events

#### World Bank Open Data
- **Coverage:** 1,600+ development indicators, 200+ countries, 1960s–present
- **RELICK use:** International macro context for global events; GDP, debt, inflation for emerging market event annotation
- **Implementation:** World Bank API is free and open

#### NBER Working Papers
- **Coverage:** National Bureau of Economic Research papers — gold standard for macro research
- **RELICK use:** Tier 3 credibility source; validates causal chains in the knowledge graph; training data for mechanism extraction
- **Implementation:** NBER provides public API and bulk download

---

### Phase 3: Pre-1979 Historical Expansion (Year 2)
*Goal: Cover the Great Depression, stagflation era, Bretton Woods, oil shocks, going back to 1900*

**Sources:**
- **NBER Macrohistory Database:** Monthly data for US going back to 1854 (interest rates, production, prices)
- **Global Financial Data:** Commercial database; consider data partnership
- **Measuring Worth:** GDP deflators, stock returns back to 1790 for US; free academic
- **Historical newspaper archives:** New York Times Archive (subscription), ProQuest Historical Newspapers
- **Digitized academic papers:** Great Depression scholarship, Friedman & Schwartz monetary history

**Strategy:** Manual curation of 500 high-impact pre-1979 events by a research team or crowdsourcing. These become extremely high-value graph nodes — events like the 1929 crash, 1971 Nixon shock, 1973 oil embargo, 1987 Black Monday with full structural annotations are rarer and more valuable than any modern event.

---

## 3. GDELT Deep Evaluation & Implementation Guide

### What GDELT Is

The Global Database of Events, Language, and Tone (GDELT) is one of the most ambitious open data projects in history:
- **Coverage:** Every news event globally since January 1979
- **Scale:** 300+ million events; 2.5+ billion event-mentions
- **Update frequency:** Every 15 minutes in real-time
- **Sources:** 100+ languages; news from every country globally
- **Cost:** Free. Entirely. No API key required. Data in CSV on Google Cloud Storage.
- **Maintained by:** Kalev Leetaru (Georgetown University)

### GDELT Data Structure

GDELT events are encoded in the **CAMEO (Conflict and Mediation Event Observations)** framework. Each GDELT event record contains:

```
GlobalEventID     # Unique event identifier
Day               # Date (YYYYMMDD)
MonthYear         # Month + Year
Year              # Year
FractionDate      # Decimal year
Actor1Code        # Country/organization of Actor 1 (e.g., USA, CHN)
Actor1Name        # Name of Actor 1
Actor1CountryCode # Country code of Actor 1
Actor2Code        # Country/organization of Actor 2
Actor2Name        # Name of Actor 2  
IsRootEvent       # 1 if this event is new, 0 if derivative
EventCode         # CAMEO event type (e.g., 0211 = Appeal for economic cooperation)
EventBaseCode     # High-level CAMEO category
EventRootCode     # Root CAMEO category
QuadClass         # 1=Verbal Cooperation, 2=Material Cooperation, 3=Verbal Conflict, 4=Material Conflict
GoldsteinScale    # -10 to +10 conflict/cooperation score
NumMentions       # Number of times event was mentioned in news
NumSources        # Number of unique sources mentioning event
NumArticles       # Number of unique articles mentioning event
AvgTone           # Average sentiment tone of articles covering event
Actor1Geo_*       # Geographic location fields for Actor 1
Actor2Geo_*       # Geographic location fields for Actor 2
SOURCEURL         # URL of source article
```

### GDELT Strengths for RELICK

1. **Real-time global coverage:** 15-minute update cycle; genuinely comprehensive
2. **Historical depth:** 1979–present, unmatched for free data
3. **CAMEO framework:** Structured event taxonomy — maps well to RELICK's event type schema
4. **GoldsteinScale:** Instant quantitative signal on event severity/direction
5. **AvgTone:** Sentiment signal from news coverage — useful for qualitative signal extraction
6. **NumMentions:** Measures how much attention an event received — proxy for market significance
7. **Multi-lingual:** Captures events from non-English sources often missed by US-centric platforms

### GDELT Weaknesses & Mitigations

| Weakness | Severity | Mitigation |
|---|---|---|
| **Very noisy at event level** | High | Use NumMentions > 10 as a minimum threshold; only promote high-attention events |
| **CAMEO codes imperfect** | Medium | Re-classify with LLM; treat CAMEO as a first-pass signal only |
| **No financial market linkage** | High | RELICK provides this — the financial linkage layer is our value add |
| **Source quality varies enormously** | High | Never use GDELT event directly; always route through Verification Agent |
| **No event body/full text** | Medium | GDELT provides source URL; fetch and parse the underlying article |
| **Duplicate events (multiple mentions)** | Medium | Deduplicate by SOURCEURL + EventCode + Day; aggregate mentions |
| **False positives in conflict events** | Medium | Use QuadClass + GoldsteinScale together; not QuadClass alone |

### GDELT Implementation

```python
# GDELT v2 real-time fetcher
import requests
import zipfile
import pandas as pd
from io import BytesIO
from datetime import datetime, timedelta

class GDELTFetcher:
    BASE_URL = "http://data.gdeltproject.org/gdeltv2/"
    MASTER_LIST = "http://data.gdeltproject.org/gdeltv2/masterfilelist.txt"
    
    FINANCIAL_CAMEO_CODES = [
        '0211',  # Appeal for economic cooperation
        '0231',  # Appeal for economic aid
        '0631',  # Demand economic sanctions
        '161',   # Reduce relations (financial context)
        '163',   # Impose sanctions or boycott
        '171',   # Coerce economically
        '173',   # Impose embargo or blockade
    ]
    
    HIGH_ATTENTION_THRESHOLD = 10  # NumMentions minimum
    SIGNIFICANT_TONE_THRESHOLD = -3.0  # AvgTone below this = strongly negative
    
    def fetch_recent(self, hours_back: int = 24) -> pd.DataFrame:
        """Fetch GDELT events from last N hours"""
        # GDELT updates every 15 minutes; fetch from master file list
        master = requests.get(self.MASTER_LIST).text.strip().split('\n')
        
        # Filter to files from last N hours
        cutoff = datetime.utcnow() - timedelta(hours=hours_back)
        recent_files = [f for f in master if self._file_datetime(f) > cutoff]
        
        events = []
        for file_url in recent_files[-4:]:  # Process last 4 files (1 hour)
            df = self._download_and_parse(file_url)
            filtered = self._filter_market_relevant(df)
            events.append(filtered)
        
        return pd.concat(events, ignore_index=True)
    
    def _filter_market_relevant(self, df: pd.DataFrame) -> pd.DataFrame:
        return df[
            (df['NumMentions'] >= self.HIGH_ATTENTION_THRESHOLD) &
            (
                (df['EventRootCode'].isin(['01', '02', '06', '16', '17'])) |  # Key CAMEO root codes
                (df['Actor1CountryCode'].isin(['USA', 'CHN', 'EU', 'GBR', 'JPN', 'DEU'])) |  # G7 actors
                (df['AvgTone'] < self.SIGNIFICANT_TONE_THRESHOLD)  # Strongly negative tone
            )
        ]
```

### GDELT → RELICK Pipeline Flow

```
GDELT 15-min update
    ↓
[Ingestion Agent] pulls and filters
    ↓
Financial relevance scoring (CAMEO + actors + tone + attention)
    ↓
Fetch source article text from SOURCEURL
    ↓
[LLM Classification] event type, affected assets, severity
    ↓
Staging table (pending verification)
    ↓
[Verification Agent] 2+ independent Tier 1-3 sources required
    ↓
[Enrichment Agent] causal links, market reactions, macro regime
    ↓
Event Knowledge Graph (live)
```

---

## 4. Credibility Scoring System Design

### The 5-Tier Framework

```
TIER 1 (Score: 1.0) — Primary Government & Regulatory Sources
║ Federal Reserve meeting transcripts, press releases, speeches
║ SEC filings (10-K, 10-Q, 8-K, proxy statements)
║ Congressional Records and testimony transcripts  
║ US Treasury official releases, debt ceiling legislation
║ IMF Article IV reports, World Bank official publications
║ NBER recession determination announcements
║ BLS official statistical releases (CPI, Employment Situation)
║ Eurostat, ONS, Statistics Canada official data releases

TIER 2 (Score: 0.85) — Tier-1 Financial Press
║ Wall Street Journal (wsj.com)
║ Financial Times (ft.com)
║ Reuters (reuters.com)
║ Bloomberg News (bloomberg.com/news)
║ Associated Press (apnews.com)
║ CNBC (primary reporting, not opinion)

TIER 3 (Score: 0.80) — Academic & Research
║ NBER Working Papers
║ Federal Reserve Bank research papers
║ Peer-reviewed journals (Journal of Finance, QJE, AER)
║ IMF Working Papers
║ BIS Working Papers

TIER 4 (Score: 0.65) — Quality General Press
║ The Economist
║ New York Times (business/economic reporting)
║ BBC News
║ Le Monde (Economie)
║ Der Spiegel
║ Nikkei Asia

TIER 5 (Score: 0.0) — EXCLUDED
╘ Anonymous sources, social media, Reddit, Twitter/X posts
╘ Opinion pieces without primary source citations
╘ Press releases from private companies (without corroboration)
╘ Substack newsletters, Seeking Alpha articles
╘ Any source without a verified institutional author
```

### Composite Score Calculation

```python
def calculate_credibility_score(
    sources: List[Source],
    cross_source_agreement: float  # 0.0-1.0 from LLM assessment
) -> float:
    """
    Composite credibility score for an event.
    Returns 0.0-1.0; events below 0.6 are not promoted to knowledge graph.
    """
    if not sources:
        return 0.0
    
    # Rule: minimum 2 independent Tier 1-3 sources required
    tier_qualified = [s for s in sources if s.tier <= 3]
    if len(tier_qualified) < 2:
        return 0.0  # Hard rejection
    
    # Component 1: Best source tier (50% weight)
    best_tier_score = max(s.tier_score for s in tier_qualified)  # e.g., 1.0 for Tier 1
    
    # Component 2: Source count bonus (30% weight)
    # Caps at 5 sources (diminishing returns beyond that)
    source_count = min(len(tier_qualified), 5)
    source_count_normalized = source_count / 5.0  # 0.2 for 1 source, 1.0 for 5+
    
    # Component 3: Cross-source agreement (20% weight)
    # LLM assesses whether all sources agree on the core facts
    
    composite = (
        best_tier_score * 0.50 +
        source_count_normalized * 0.30 +
        cross_source_agreement * 0.20
    )
    
    return round(composite, 3)

# Example: WSJ + Reuters corroborating, strong agreement
# = (0.85 * 0.50) + (0.4 * 0.30) + (0.95 * 0.20)
# = 0.425 + 0.120 + 0.190 = 0.735

# Example: Fed transcript + WSJ + FT, strong agreement  
# = (1.0 * 0.50) + (0.6 * 0.30) + (0.98 * 0.20)
# = 0.500 + 0.180 + 0.196 = 0.876
```

### Source Publisher Registry

RELICK maintains a curated publisher registry (`source_publishers` table) with:

```sql
CREATE TABLE source_publishers (
    publisher_id    UUID PRIMARY KEY,
    name            VARCHAR(255) NOT NULL,
    domain          VARCHAR(255) UNIQUE NOT NULL,
    credibility_tier INTEGER NOT NULL CHECK (credibility_tier BETWEEN 1 AND 5),
    tier_score      DECIMAL(3,2) NOT NULL,
    geographic_focus VARCHAR(50),  -- 'US', 'EU', 'Global', etc.
    language        VARCHAR(10) DEFAULT 'en',
    is_active       BOOLEAN DEFAULT TRUE,
    notes           TEXT,
    created_at      TIMESTAMPTZ DEFAULT NOW()
);
```

New publishers are added manually by the RELICK team. The registry is conservative by design — it's easier to add sources than to rebuild trust after a bad source contaminates the graph.

---

## 5. Event Knowledge Graph Schema

### Core Tables

#### `events` (Primary Event Table)

```sql
CREATE TABLE events (
    -- Identity
    event_id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    external_id         VARCHAR(255),           -- GDELT GlobalEventID or EDGAR accession number
    
    -- Classification
    title               VARCHAR(500) NOT NULL,
    event_type          event_type_enum NOT NULL,  -- See enum below
    event_subtype       VARCHAR(100),
    severity            INTEGER CHECK (severity BETWEEN 1 AND 10),
    geographic_scope    TEXT[],                 -- Array: ['US', 'EU', 'Global']
    primary_actors      JSONB,                  -- [{name, type, country, role}]
    tags                TEXT[],                 -- Searchable tags
    
    -- Content
    summary             TEXT NOT NULL,          -- 200-400 word LLM-generated summary
    mechanism           TEXT,                   -- How this event affects markets (mechanism explanation)
    historical_context  TEXT,                   -- What preceded this; broader context
    
    -- Temporal
    event_date          DATE NOT NULL,
    event_date_end      DATE,                   -- For multi-day events
    event_date_precision VARCHAR(20) DEFAULT 'day', -- 'exact', 'day', 'week', 'month'
    
    -- Market Context at Event Time
    macro_regime        JSONB NOT NULL,          -- See macro_regime schema below
    
    -- Market Reactions (post-event price data)
    market_reactions    JSONB NOT NULL,          -- See market_reactions schema below
    
    -- Credibility
    credibility_score   DECIMAL(4,3) CHECK (credibility_score >= 0.6),  -- Min 0.6 to enter graph
    verification_status VARCHAR(20) DEFAULT 'pending',  -- 'pending', 'verified', 'rejected'
    
    -- Embeddings (stored as vector)
    embedding_small     vector(1536),           -- text-embedding-3-small (fast search)
    embedding_large     vector(3072),           -- text-embedding-3-large (precision matching)
    
    -- Metadata
    created_at          TIMESTAMPTZ DEFAULT NOW(),
    last_enriched       TIMESTAMPTZ,
    enrichment_version  INTEGER DEFAULT 1,
    is_manually_curated BOOLEAN DEFAULT FALSE,
    curator_notes       TEXT
);

-- Event type enum
CREATE TYPE event_type_enum AS ENUM (
    'monetary_policy',       -- Fed decisions, central bank actions globally
    'fiscal_policy',         -- Government spending, tax policy, debt
    'geopolitical',          -- Wars, elections, diplomatic crises, sanctions
    'corporate',             -- Earnings, mergers, bankruptcies, IPOs
    'macroeconomic',         -- GDP, inflation, employment releases
    'financial_crisis',      -- Bank failures, credit events, systemic risk
    'regulatory',            -- Laws, regulations, court decisions
    'natural_disaster',      -- Hurricanes, earthquakes, pandemics
    'commodity_shock',       -- Oil embargoes, supply disruptions
    'technological',         -- Major tech innovations with market impact
    'market_structure'       -- Flash crashes, circuit breakers, exchange events
);
```

#### `event_sources` (Source Citations)

```sql
CREATE TABLE event_sources (
    source_id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    event_id            UUID NOT NULL REFERENCES events(event_id) ON DELETE CASCADE,
    publisher_id        UUID NOT NULL REFERENCES source_publishers(publisher_id),
    
    -- Citation Details
    title               VARCHAR(1000) NOT NULL,
    url                 VARCHAR(2000) NOT NULL,
    published_date      DATE,
    author              VARCHAR(500),
    excerpt             TEXT,                   -- Key quote/passage used
    
    -- Quality
    url_verified        BOOLEAN DEFAULT FALSE,  -- URL still returns 200
    last_url_check      TIMESTAMPTZ,
    
    created_at          TIMESTAMPTZ DEFAULT NOW()
);
```

#### `event_edges` (Causal Graph Edges)

```sql
CREATE TABLE event_edges (
    edge_id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    source_event_id     UUID NOT NULL REFERENCES events(event_id),
    target_event_id     UUID NOT NULL REFERENCES events(event_id),
    
    edge_type           VARCHAR(50) NOT NULL,   -- 'caused', 'triggered', 'amplified', 'resolved', 'related'
    direction           VARCHAR(20) NOT NULL,   -- 'causal', 'temporal', 'thematic'
    confidence          DECIMAL(3,2),           -- 0.0-1.0 confidence in this causal link
    lag_days            INTEGER,                -- How many days after source did target occur?
    description         TEXT,                   -- LLM-generated explanation of the relationship
    
    created_at          TIMESTAMPTZ DEFAULT NOW()
);

-- Index for graph traversal
CREATE INDEX idx_event_edges_source ON event_edges(source_event_id);
CREATE INDEX idx_event_edges_target ON event_edges(target_event_id);
```

#### `parallel_matches` (Cached Similarity Results)

```sql
CREATE TABLE parallel_matches (
    match_id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    query_hash          VARCHAR(64) NOT NULL,   -- SHA-256 of normalized query
    
    -- Query context
    query_text          TEXT NOT NULL,
    query_signals       JSONB NOT NULL,         -- Extracted quantitative + qualitative signals
    
    -- Results
    top_matches         JSONB NOT NULL,         -- [{event_id, confidence_score, rank, memo_excerpt}]
    full_memo           TEXT NOT NULL,          -- Generated analyst memo
    
    -- Cache management
    created_at          TIMESTAMPTZ DEFAULT NOW(),
    expires_at          TIMESTAMPTZ DEFAULT NOW() + INTERVAL '7 days',
    hit_count           INTEGER DEFAULT 0       -- Cache hits; reuse for similar queries
);

CREATE INDEX idx_parallel_matches_query ON parallel_matches(query_hash);
CREATE INDEX idx_parallel_matches_expires ON parallel_matches(expires_at);
```

#### `user_intel_submissions` (Community Data)

```sql
CREATE TABLE user_intel_submissions (
    submission_id       UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id             UUID NOT NULL,          -- References users table
    
    -- Submission
    input_text          TEXT NOT NULL,
    input_type          VARCHAR(20),            -- 'headline', 'thesis', 'question'
    
    -- System output
    parallel_match_id   UUID REFERENCES parallel_matches(match_id),
    
    -- Anonymized aggregate signals (for Oracle community layer)
    extracted_signals   JSONB,                  -- Quantitative + qualitative signals (no PII)
    is_public_signal    BOOLEAN DEFAULT FALSE,  -- User opted to share anonymized signal
    
    created_at          TIMESTAMPTZ DEFAULT NOW()
);
```

### Vector Indexes

```sql
-- HNSW index for fast ANN search (pgvector)
CREATE INDEX idx_events_embedding_small 
    ON events USING hnsw (embedding_small vector_cosine_ops)
    WITH (m = 16, ef_construction = 64);

CREATE INDEX idx_events_embedding_large
    ON events USING hnsw (embedding_large vector_cosine_ops)
    WITH (m = 16, ef_construction = 64);
```

---

## 6. Real-Time Monitoring Pipeline

### Pipeline Architecture

```
Live Data Sources (every 15 minutes)
├── GDELT v2 real-time feed
├── NewsAPI live endpoint  
├── SEC EDGAR EDGAR Online RSS
├── Federal Reserve RSS feeds
└── World Bank country alerts
         ↓
[Celery Beat Scheduler] triggers every 15 minutes
         ↓
[Ingestion Agent] — filters, classifies, stages
         ↓
[Redis Queue] — staged events queued for verification
         ↓
[Verification Agent] — source cross-reference
    ├── Promoted to knowledge graph (verified)
    └── Rejected (insufficient sources) — logged for audit
         ↓
[Enrichment Agent] — causal links, market reactions, embeddings
         ↓
[Live Event Store] (Postgres events table)
         ↓
[Oracle Alert Engine] — compares new event to active parallel watches
         ↓
[Novu Notification Service] — push/email to Oracle subscribers
```

### Oracle Parallel Alert Logic

```python
class ParallelAlertEngine:
    ALERT_THRESHOLD = 0.75  # Confidence score to trigger alert
    
    async def check_new_event(self, new_event: Event):
        """Check if new event triggers any Oracle subscriber's parallel watches"""
        
        # Get all active Oracle users' watch signals
        active_watches = await db.get_active_parallel_watches()
        
        for watch in active_watches:
            # Fast vector similarity check
            similarity = cosine_similarity(
                new_event.embedding_small,
                watch.pattern_embedding
            )
            
            if similarity > self.ALERT_THRESHOLD:
                # Full confidence score calculation
                confidence = await self.calculate_full_confidence(
                    new_event, watch.historical_pattern
                )
                
                if confidence >= self.ALERT_THRESHOLD:
                    await self.send_alert(
                        user_id=watch.user_id,
                        message=f"Pattern match detected: {new_event.title} "
                               f"matches your {watch.pattern_name} parallel "
                               f"(confidence: {confidence:.0%})",
                        event_id=new_event.event_id,
                        confidence=confidence
                    )
```

---

## 7. Data Partnership Opportunities

### Tier A: Strategic Data Partnerships (Pursue in Year 1)

**1. Polygon.io**  
- *What they provide:* Real-time + historical tick-level market data; clear commercial ToS; $29–$999/mo
- *Partnership angle:* RELICK is a potential showcase customer; Polygon benefits from being featured as RELICK's data provider to institutional customers
- *Ask:* Startup-friendly pricing in exchange for logo placement and attribution in the product

**2. NBER (National Bureau of Economic Research)**  
- *What they provide:* The authoritative source for recession dates, macro research; used in academic citations globally
- *Partnership angle:* Academic/research alignment; NBER's data is already free but a formal partnership adds credibility signal
- *Ask:* Formal citation agreement; potential to integrate NBER recession indicator data directly

**3. Federal Reserve Education Resources**  
- *What they provide:* Free educational content; data legitimacy
- *Partnership angle:* RELICK's Explorer tier is a financial education product; the Fed actively promotes financial literacy
- *Ask:* Recognition as a Fed-compatible educational tool; potentially joint content

### Tier B: Growth-Stage Partnerships (Year 2)

**4. Refinitiv / LSEG**  
- *What they provide:* Comprehensive historical data; news archive going back decades
- *Why interesting:* LSEG is competing with Bloomberg and needs differentiated products; a RELICK integration into their data ecosystem could be mutually valuable
- *Ask:* Discounted data licensing in exchange for RELICK recommending/integrating LSEG data

**5. FactSet or S&P Global Capital IQ**  
- *What they provide:* Clean, structured fundamental data; earnings history; analyst estimates
- *Why interesting:* Either could be an acquirer or a data partner; early relationship building matters
- *Ask:* Startup data access program; both have one

**6. Quandl / Nasdaq Data Link**  
- *What they provide:* Alternative data marketplace; structured datasets
- *Why interesting:* RELICK's event knowledge graph could eventually be sold through Quandl's marketplace, creating a revenue line
- *Ask:* Listing RELICK event data as a Quandl dataset (Year 2+)

### Tier C: Monetization Partnerships (Year 3+)

**7. Selling the knowledge graph as a data product**  
Once the event knowledge graph reaches 100,000+ verified events:
- License to academic institutions: $5,000–25,000/year
- License to hedge funds and family offices: $25,000–$100,000/year
- License to data aggregators (Quandl, Bloomberg): $50,000–$500,000/year
- API access for institutional developers: $999–$4,999/month

---

## 8. 5-Year Data Moat Building Strategy

### Year 1: Foundation Layer (2026)
**Milestone:** 10,000 verified events; full 1990–2024 coverage; real-time pipeline live

- Complete ingestion of all FRED macro series
- 2,000+ manually reviewed high-impact events (1990–2024) seeded by research team
- Automated pipeline bringing in 50–100 new verified events per week
- Credibility scoring system calibrated
- GDELT pipeline live with quality filtering
- Average credibility score of graph: 0.80+

**Moat status:** Early. The graph is useful but replicable with 6–12 months of engineering effort.

### Year 2: Structural Depth (2027)
**Milestone:** 50,000 verified events; pre-1979 historical expansion begins; first data licensing conversation

- Historical expansion: 500 key events pre-1979, going back to 1929 crash
- Causal graph edges: 200,000+ edges connecting events into a true graph structure
- User intel submissions: 50,000+ anonymized submissions becoming proprietary signal
- First institutional data partner (academic or research fund)
- Community consensus data: Oracle users' parallel watches aggregate into unique intelligence

**Moat status:** Growing. The causal graph structure and user-generated signals are now proprietary and impossible to replicate without RELICK's user base.

### Year 3: Data Network Effects (2028)
**Milestone:** 250,000 verified events; proprietary signal from user behavior; acquisition conversations possible

- Event graph covers every material market event globally since 1900 (for major markets)
- User-generated intel layer: 500,000+ submissions; aggregate signals are proprietary market intelligence
- Community divergence data: What Oracle users are watching vs. what history suggests — unique dataset
- API revenue: 10–20 institutional API subscribers paying $999–$4,999/month
- Graph completeness: <5% gap rate on major S&P moves >2% since 1980

**Moat status:** Deep. No competitor can replicate the combination of verified historical depth + real-time enrichment + user signal layer. Acquisition value is primarily the graph.

### Year 4: Proprietary Intelligence Layer (2029)
**Milestone:** The user behavior layer IS the product; knowledge graph is a technical foundation

- Oracle user "watches" aggregated into a live market intelligence layer: what 10,000 serious investors are watching this week
- Divergence alerts: When community consensus diverges from historical baseline — a signal itself
- Pattern frequency tracking: Which historical patterns are being matched most in current market conditions (itself a sentiment indicator)
- First enterprise white-label contract: A financial media company or research firm licensing RELICK's graph and UI

### Year 5: Institutional Standard (2030)
**Milestone:** RELICK is cited in financial media; the knowledge graph is referenced in investment research

- 2M+ events; comprehensive global coverage 1900–2030
- The "RELICK Confidence Score" is referenced in investment memos the way the VIX is referenced in volatility discussions
- Bloomberg or LSEG is in serious acquisition conversation or the company is preparing for Series B/C
- The moat is now structural: RELICK's graph is cited by academics, referenced by financial press, and integrated into professional workflows

### Why This Moat Is Durable

1. **Time-based compounding:** The longer the pipeline runs, the more complete the coverage. A competitor starting today is always 5 years behind.
2. **Causal graph structure:** The edges between events (causality, not just co-occurrence) cannot be computed retroactively at scale. They require the enrichment pipeline running on each event at time of ingestion.
3. **User signal data:** 500,000+ anonymized intel submissions are genuinely proprietary. No one else has this dataset.
4. **Credibility verification:** The 5-tier system and source registry were built over years. Rebuilding trust after any contamination takes years more.
5. **Network effects:** The more Oracle users, the better the community consensus layer. The better the community consensus, the more valuable Oracle is. Classic network effect lock-in.

---

## 9. Advisory Notes

> *Issues with the data strategy that will affect product quality, operational reality, and moat building timelines.*

---

### 9.1 The Verification Agent Running Every 2 Hours Is Too Slow for a Real-Time Intelligence Product

The pipeline as designed: GDELT detects an event → Ingestion Agent stages it → Verification Agent picks it up in the next 2-hour cycle → Enrichment Agent processes it → Oracle alert fires.

In practice: a market-moving event (e.g., an emergency Fed rate decision) happens at 10am. It enters the knowledge graph at earliest by noon, more likely by 2pm. Oracle alerts fire in the early afternoon for something that happened in the morning.

For a product positioning itself as delivering real-time parallel intelligence to paying subscribers, 2–4 hour detection-to-alert latency is not competitive. Financial Twitter will have discussed the event exhaustively before RELICK's alert arrives.

**What to do:** Make verification event-driven, not batch-scheduled. When a high-urgency event (GoldsteinScale < -5, NumMentions > 100) enters the staging table, trigger immediate asynchronous verification rather than waiting for the 2-hour Celery beat. Redis pub/sub or a Celery priority queue can handle this. For most events, the 2-hour batch is fine. For market-moving events, it needs to be sub-30 minutes.

---

### 9.2 The "2,000 Manually Reviewed Events by Month 6" Milestone Requires ~1,000 Hours of Research Labor

Seeding the knowledge graph with 2,000 high-quality events by Month 6 is presented as a data pipeline milestone. It's actually a human labor milestone.

At 30 minutes per event (researching, verifying 2+ sources, writing a 200-word summary, tagging causal links, calculating market reactions) — 2,000 events = 1,000 hours = 25 full-time weeks of work for one person, or 4–5 months of parallel work for 2 people doing nothing else.

The founding team doesn't have 1,000 hours of research time in Months 1–6 while also building the platform.

**What to do:** Reduce the Month 6 seed target to 200–500 events covering the highest-demand historical periods (2008, 2020, 2022, 1987, 1997). Focus on depth over breadth initially. Automate more aggressively: the Enrichment Agent should produce a draft summary that a human reviews in 10 minutes, not 30. Consider a contractor research team (2–3 history/finance students at $20/hour) for the initial seeding sprint. 200 excellent events beats 2,000 mediocre ones.

---

### 9.3 The Credibility Score Minimum of 0.6 Is Arbitrary and Uncalibrated

The hard floor of 0.6 for knowledge graph entry was chosen without a calibration basis. There is no analysis showing what a 0.6-credibility event actually looks like in practice and whether it's good enough to appear in user-facing outputs.

The math: A single WSJ article with no other corroboration scores:
- best_tier_score × 0.50 = 0.85 × 0.50 = 0.425
- source_count_normalized × 0.30 = 0.20 × 0.30 = 0.060
- cross_source_agreement × 0.20 = 0.90 × 0.20 = 0.180
- **Total: 0.665** — above the 0.6 floor, enters knowledge graph

A single WSJ article with no corroboration entering the knowledge graph as a verified event is a problem. WSJ makes mistakes. WSJ publishes contested interpretations. The 0.6 floor allows single-source events from Tier 2 outlets.

**What to do:** Raise the floor to 0.70 or enforce a hard rule that at least 2 independent sources must corroborate regardless of the composite score. The formula already has a `len(tier_qualified) < 2` hard rejection — make sure this is actually enforced in the implementation before the composite score check, not after. Calibrate the threshold against a holdout set of known events.

---

### 9.4 The Federal Reserve "Partnership" Idea Is Naive

The document suggests pursuing "Federal Reserve Education Resources" as a Tier A partnership, specifically: "Recognition as a Fed-compatible educational tool; potentially joint content."

The Federal Reserve does not endorse commercial financial products. Ever. The Federal Reserve Bank of San Francisco, NY Fed, and Board of Governors have strict policies against endorsing, co-branding, or appearing to validate specific commercial software or platforms.

Pursuing a "Fed partnership" will result in either a polite no or silence. More importantly, if RELICK uses language implying Fed endorsement without it, the legal risk is real (Section 709 of Title 18 USC prohibits false suggestion of Fed affiliation).

**What to do:** Remove this from the partnership roadmap. The correct framing is: "RELICK uses Federal Reserve public data (FOMC transcripts, Beige Book, speeches) as Tier 1 primary sources." That's accurate, legally safe, and valuable to communicate. A formal partnership with the Fed is not achievable and not necessary.

---

### 9.5 Pre-1979 Historical Events Should Be in MVP, Not Year 2

The document puts pre-1979 event coverage (1929 crash, 1971 Nixon shock, 1973 oil embargo, 1987 Black Monday) in Phase 3 / Year 2. This is backwards.

These events — the 1929 crash, the 1971 closing of the gold window, the 1973 OPEC oil embargo, the 1987 Black Monday — are the most-requested historical parallels by serious investors. When someone submits "the Fed is losing control of inflation," the most powerful parallel is not 2022. It's 1972–1975. When someone submits "we're entering a bear market," the most credible parallel is 1929 or 1987, not 2000.

A parallel engine that can only match to events post-1979 will miss the most historically resonant comparisons every time. Users will immediately notice and comment on it.

**What to do:** Prioritize curating 50–100 pre-1979 "landmark" events manually for MVP. These 50 events — thoroughly annotated, deeply sourced, covering the key inflection points of 20th century economic history — are more valuable than 5,000 automatically generated post-2000 events. They are the intellectual anchor of the product. Manual curation is the right approach for this tier.

---

### 9.6 The 5-Year Moat Claim Contains One Partially False Premise

The document states: "The edges between events (causality, not just co-occurrence) cannot be computed retroactively at scale. They require the enrichment pipeline running on each event at time of ingestion."

This is not fully accurate. A competitor with sufficient compute and the same source data (FRED, GDELT, EDGAR) could in principle run RELICK's entire enrichment pipeline retroactively on historical events. The bottleneck is data access and compute time, not a fundamental impossibility. A well-funded team (say, S&P Global with a $3M engineering budget) could plausibly rebuild 10 years of RELICK's knowledge graph in 12–18 months.

**The moat that is actually irreplicable:** User behavior data. The 500,000+ intel submissions — what specific headlines serious investors were worried about, which parallels they searched for, at what point in market cycles — this cannot be reconstructed. It's a unique behavioral dataset that exists only if RELICK has users actively submitting queries over years.

**What to do:** Reframe the moat story: the graph structure is a 2–3 year head start; the user behavior layer is the permanent moat. This is a more defensible and honest claim, and it changes how you talk about network effects to investors.
