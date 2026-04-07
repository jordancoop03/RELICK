# RELICK — Advisory Synthesis

> **Version:** 1.0 | **Date:** April 2026  
> **Purpose:** The decision document. Cross-cutting advisory synthesized from all 7 core documents. Read this before making any major strategic decision.  
> **Tone:** Direct. Unfiltered. Nothing is set in stone.

---

## Table of Contents
1. [The Single Unified Recommendation](#1-the-single-unified-recommendation)
2. [What the Research Actually Shows vs. What We've Assumed](#2-what-the-research-actually-shows-vs-what-weve-assumed)
3. [Final Pricing & Tier Structure Recommendation](#3-final-pricing--tier-structure-recommendation)
4. [The Single Biggest Risk](#4-the-single-biggest-risk)
5. [The Single Biggest Opportunity](#5-the-single-biggest-opportunity)
6. [Honest Overall Assessment](#6-honest-overall-assessment)
7. [90-Day Priority Action List](#7-90-day-priority-action-list)

---

## 1. The Single Unified Recommendation

> **Build the parallel matching engine first. Validate that it feels genuinely surprising to users. Everything else — the pricing, the fundraising, the marketing, the roadmap — is contingent on that single product truth.**

Every document in this repository is well-reasoned, internally consistent, and based on solid research. And every document shares the same critical flaw: it is written as if product-market fit is already known. It isn't.

RELICK's entire thesis rests on one empirical claim that has not been tested: that a serious investor, upon seeing their submitted headline matched to a non-obvious historical parallel with a sourced analyst memo, will experience something like: *"I didn't see it that way before. This changes how I'm thinking about this."* That moment — the re-taste — is the product. If it doesn't happen reliably, nothing else in these documents matters.

So the unified recommendation is this: before finalizing pricing, before approaching investors, before writing a single line of infrastructure code that isn't required to run the core engine — **build the Submit Intel pipeline end-to-end with 200 manually curated events and test it with 30 real users**. Measure whether the outputs surprise them. Measure whether they'd pay. Then, and only then, commit to the rest of the plan.

If the answer is yes: the financial projections are achievable, the fundraise is fundable, and the moat thesis is real. Build everything.

If the answer is no: you have a 6-month head start on finding the right product form before spending two years on the wrong one.

---

## 2. What the Research Actually Shows vs. What We've Assumed

### Things the Research Confirms

**The market gap is real.** After scanning every competitor — Bloomberg, Hedgeye, 42Macro, Koyfin, TradingView, Macro Hive, Quill, AlphaSense — no company offers AI-powered historical parallel matching with verified, credibility-scored source backing at $29–$99/month. This gap exists because it requires a combination of capabilities (structured knowledge graph + LLM matching + credibility infrastructure + financial data integration) that no single team has executed together. The gap is not due to lack of demand.

**The price band is validated.** TradingView Plus at $29.95/month has millions of subscribers. Hedgeye Macro Show at $99/year (intro) has a substantial following. Koyfin Plus at $39/month is growing. These are real data points showing that serious retail investors pay in this range for financial tools. RELICK's pricing is calibrated correctly.

**The acquisition thesis has precedent.** S&P Global paid $550M for Kensho. Bloomberg has made ~$990M in data acquisitions. LSEG paid $27B for Refinitiv. These are not hypothetical acquirers — they are demonstrated buyers of AI financial intelligence assets. A structured event knowledge graph with verified source provenance and a consumer brand is exactly what each of them lacks and would pay for.

**The content-led distribution is trackable.** The "Today in Market History" format works — it exists in rudimentary form already on Twitter/X and generates consistent engagement. RELICK's differentiated version (annotated charts + sourced mechanism + asset class implications) is meaningfully better than what's currently out there. This is a real distribution channel.

### Things We've Assumed Without Evidence

**The 15M serious retail investors figure.** This is an estimate with no cited source. The actual number paying for *any* research subscription today is likely 2–4M. The TAM is real; the specific population size is not validated. Model conservatively.

**The $29–$99 price band is "uncontested."** It isn't. Koyfin's $39/month Plus tier is a direct budget competitor for the same user. The band is uncontested for RELICK's *specific value proposition* — not for the user's subscription wallet.

**Year 1 subscriber projections.** 60 Analyst subscribers in Q1 is highly optimistic for a cold launch. A conservative but still fundable Year 1 is 150–200 Analyst subscribers and 30–40 Oracle subscribers by EOY. That's still a fundable story.

**The CAC of $8–15 at launch.** Content-led CAC is Year 2+. Launch CAC is 3–5x this. Model $60–100 blended for the first 90 days.

**The credibility scoring calibration.** The 0.6 floor and the composite formula weights are intuitive choices, not empirically calibrated ones. They may be wrong. This matters because the knowledge graph's quality is the product.

**That users know what they want.** No customer discovery interviews are documented anywhere in this document set. The product is being built based on the founding team's theory of what serious investors need. This is the most important assumption of all, and it hasn't been tested.

---

## 3. Final Pricing & Tier Structure Recommendation

Based on live competitor pricing data (scraped April 2026), SaaS conversion benchmarks, and the advisory analysis across all documents, the recommended final pricing is:

### Keep: The Three-Tier Structure

Explorer → Analyst → Oracle is the right architecture. Three tiers work because each tier has a distinct value driver:
- **Explorer:** Introduces the concept; drives word-of-mouth; free acquisition
- **Analyst:** Core revenue at volume; Submit Intel is the paywall feature
- **Oracle:** High-retention power users; live monitoring + community is the differentiation

### Change: Remove Credits from Analyst Tier

**Current:** 50 credits/month with rollover (shared across all Analyst features)

**Recommended:** Unlimited chart annotations and Highlight-to-Learn. Submit Intel limited to **20 queries/month** (not credits — specific queries). Oracle is unlimited Submit Intel.

**Why:** Credit systems create anxiety, reduce engagement, and increase churn. The Analyst-to-Oracle differentiation should be about what you can *do* (live monitoring, community consensus, unlimited Submit Intel), not how much you can do. Engagement is the retention mechanism. Remove anything that reduces it on the Analyst tier.

### Change: Fix the Annual Pricing Discounts

**Current:** Analyst $290/year (17% discount), Oracle $890/year (25% discount)

**Recommended:** Analyst $249/year (~29% discount), Oracle $799/year (~33% discount)

**Why:** At 17–25% discount, most subscribers will stay monthly — the discount isn't compelling enough to change behavior. Industry best practice for financial SaaS is 30–40% annual discount to shift cash flow. Getting subscribers onto annual contracts early dramatically improves runway. The $249/year Analyst number also has a cleaner cognitive feel.

### Change: Launch Pricing

**Current plan:** First 500 Analyst subscribers locked at $19/month for life

**Recommended:** First 500 subscribers get **6 months Oracle free**, then convert to $29/month Analyst at renewal

**Why:** The $19/month lifetime lock permanently caps LTV of your most loyal users and blocks their Oracle upgrade path. The 6-month Oracle free approach:
- Removes the price barrier for early adopters (urgency = limited quantity, not permanent discount)
- Exposes early users to Oracle features (increases long-term upgrade rate)
- Converts them to the regular Analyst price point (sustainable LTV)
- Feels more generous than a $10/month discount that lasts forever

### Keep: The $29/$99 Core Price Points

These are correct. They are market-validated by competitor data and sit in a genuine gap between free tools and professional research subscriptions. Do not change them.

### Add: A Journalist/Media Tier

**New:** $149/month or $1,199/year for financial journalists/researchers

This tier unlocks unlimited Submit Intel, unlimited exports, and citation-formatted source references. Target: newsrooms at Bloomberg Opinion, FT, WSJ, Barron's. Even 10 subscribing publications generates $14,900/month and generates the most valuable distribution possible: published articles citing RELICK.

---

## 4. The Single Biggest Risk

> **The parallel matching engine surfaces matches that feel obvious or generic — and users churn after 2–3 sessions.**

This risk is not in the Risk Registry. It is the most important risk in the company.

Every other risk in the registry — Bloomberg competing, Yahoo Finance ToS, GDELT noise, regulatory classification, data quality errors — is either unlikely or mitigable with engineering effort and capital. This risk is different. If the Submit Intel output feels like something a user could get by Googling "what happened in 1970s inflation" — if the parallels feel like confirmation of what people already know rather than genuinely illuminating insight — the product doesn't work. Period.

The product lives or dies on one subjective user experience: *"I didn't see it that way before."*

**Why this is the hardest problem:** There is no engineering solution for "not obvious enough." You can't A/B test your way to genuine insight. The parallel matching quality depends on the depth of the knowledge graph (which takes time), the sophistication of the dual-signal matching (which requires calibration), and — most importantly — whether the founding team has the intellectual judgment to curate events and design prompts that produce surprising outputs rather than safe, defensible ones. This is a human judgment problem. It requires someone on the team with deep financial market knowledge and the confidence to say "this match is interesting" and "this match is lazy."

**What to do:** Before writing a single line of production infrastructure, manually build a 50-event knowledge graph covering your 10 highest-demand historical scenarios. Run Submit Intel queries against it with 10 real users (friends who invest seriously). Watch their faces. Listen to their language. If 7 of 10 say some version of "interesting, I hadn't thought of it that way" — build everything. If 7 of 10 say "yeah, I knew that" — you have a design problem to solve before committing to the full architecture.

---

## 5. The Single Biggest Opportunity

> **The Dead Reckoning Score — built as a public, weekly published signal — is RELICK's path to becoming a media brand rather than a SaaS product, and that distinction is worth hundreds of millions of dollars in exit value.**

A SaaS product with 50,000 subscribers is a nice business. A media brand that is cited by the Wall Street Journal, referenced in Congressional testimony, and used as a shorthand for market historical risk — that is a Bloomberg-level asset.

The Dead Reckoning Score is the mechanism that transforms RELICK from a SaaS subscription into a financial media brand. Published every Monday morning. Backed by RELICK's knowledge graph. Updated with live macro data. Cited by financial journalists who don't want to spend 3 hours doing the historical research themselves.

This is not a Year 2 idea. This is a Month 9 initiative. Here's why:

1. **It costs almost nothing to build.** The infrastructure already exists — it's a weighted synthesis of regime data (from FRED) and active parallels (from the knowledge graph). One additional computation on top of existing outputs.

2. **It generates free press.** A weekly published market intelligence signal from a new company is inherently newsworthy. One Bloomberg or FT mention generates more brand awareness than $100,000 in newsletter sponsorships.

3. **It gives Oracle users a reason to stay.** A user who tracks the Dead Reckoning Score weekly as part of their market process is not churning. It becomes a habit. Habits are the ultimate retention mechanism.

4. **It is the acquisition signal.** Bloomberg and S&P Global will notice when "the RELICK Dead Reckoning Score" appears in third-party analysis. They will track it. And when the time comes, they will value not just the knowledge graph but the media brand that has built public trust in it.

Build the Dead Reckoning Score by Month 9. Publish it publicly (not just to Oracle users). Make it citeable. This single decision could double RELICK's exit value.

---

## 6. Honest Overall Assessment

### What RELICK Has Going For It

The thesis is intellectually honest. The problem is real — the gap between what institutional investors have and what serious retail investors can access is genuine and not closing on its own. The technology is ready — five years ago this product required $50M in infrastructure and a research team of 30. Today it requires two great engineers, OpenAI, FRED, and GDELT. The price band is validated. The acquisition thesis has precedent. The brand concept (the elephant, the sensory memory, the verb) is distinctive and memorable. The founding documents show a team that thinks rigorously about both product and market.

### What RELICK Has Working Against It

None of the core assumptions have been validated with actual users. The financial projections assume a flywheel that doesn't exist yet. The document set is comprehensive and detailed — which is excellent for thinking — but no amount of strategic documentation substitutes for 30 conversations with real investors who tell you what they actually need. The founding team doesn't appear to have done that yet.

More specifically: the product quality risk (parallel match accuracy) is not in the risk register and has no mitigation strategy. The moat timeline is compressed relative to what's achievable by a well-funded competitor. The launch burn rate is underestimated. The launch subscriber projections are optimistic. The comparable raises in the fundraising document don't actually compare.

### The Honest Verdict

**RELICK is a real company with a fundable thesis, a genuine market gap, and a technically buildable product. It is not yet a validated business. The gap between "fundable thesis" and "validated business" is exactly where most startups die.**

The path forward is clear: validate the core product experience before committing to the full plan. Run the 30-user experiment. If the parallel engine delivers genuine insight, execute the rest of this plan with confidence — it's a good plan. If it doesn't, you'll know in 8 weeks rather than 18 months.

The biggest risk is not Bloomberg. It's not regulation. It's not the data pipeline. The biggest risk is building a very impressive, very expensive version of a product that turns out to not be surprising enough to retain users. The documents in this repository are detailed and thoughtful. They are also entirely written from the inside. Get outside feedback before they become commitments.

---

## 7. 90-Day Priority Action List

*These are the 10 things the founding team should do in the next 90 days, in priority order. Not a nice-to-have list. A must-complete list.*

---

### Priority 1: Build the Submit Intel Prototype (Days 1–21)

**What:** A working end-to-end Submit Intel prototype with 50–100 manually curated events covering the 10 highest-demand historical scenarios (1929 crash, 1973 oil embargo, 1987 Black Monday, 1997 Asian crisis, 1998 LTCM, 2000 dot-com bust, 2008 GFC, 2011 European debt crisis, 2020 COVID crash, 2022 Fed tightening cycle).

**Why first:** This is the core product. Everything else is built on whether this works. You cannot validate pricing, fundraising readiness, or go-to-market strategy without knowing if the engine produces genuinely useful output.

**What success looks like:** 50 manually curated events, the extraction + embedding + matching + memo pipeline running end-to-end, a basic UI that lets you input a headline and receive a memo. It doesn't need to be beautiful. It needs to work.

---

### Priority 2: Run 30 User Interviews (Days 14–45)

**What:** Structured 30-minute interviews with 30 people who match the target profile: serious retail investor, actively manages $50K+, pays for at least one financial subscription (TradingView, Koyfin, Seeking Alpha, anything).

**Why now:** Before committing to infrastructure, before fundraising, before any content investment — you need to know what these users do today when they want historical context, whether they'd pay $29/month for RELICK's output, and — most importantly — whether the prototype output feels surprising and useful or obvious and generic.

**What to measure:** NPS proxy (1–10 "how likely would you recommend"), willingness to pay ("would you pay $29/month for this?"), surprise score ("did you learn something you didn't already know?"), and current tools used (how to position vs. what they already have).

**What success looks like:** 20 of 30 users give surprise score >7/10. 15 of 30 say yes to $29/month. NPS proxy >50. If these numbers come in, proceed with full confidence. If not, learn why and adjust before building.

---

### Priority 3: Incorporate and Establish Legal Foundation (Days 1–30)

**What:** Delaware C-Corp incorporation, cap table setup on Carta, IP assignment agreements for all founders, NDA template, securities law review of the product (specifically the Oracle alert feature and the analyst memo framing).

**Why now:** Legal structure needs to be in place before any investor conversation, any equity grant, or any revenue. The securities law review of the product specifically is non-negotiable before launch — the Oracle alert regulatory question needs a written legal opinion, not an assumption.

**Budget:** $5,000–15,000 for incorporation + basic legal setup. $10,000–25,000 for securities law review. Non-negotiable spend.

---

### Priority 4: Build the Waitlist to 5,000 (Days 1–90)

**What:** Launch a waitlist landing page with the core RELICK value proposition, begin publishing "Today in Market History" content on Twitter/X, LinkedIn, and Substack, seed in finance Reddit communities, and target 5,000 email sign-ups by Day 90.

**Why this timeline:** The waitlist is both a validation signal (does the content resonate?) and a distribution asset (launch converts faster from a warm list). The content also sharpens the product vision — writing the "why did [event] happen" stories forces the team to confront what the knowledge graph actually needs to contain.

**What success looks like:** 5,000 waitlist subscribers by Day 90, with >40% open rate on confirmation email (indicates genuine interest, not passive sign-ups). If you can't get 5,000 qualified people interested in the free product, revisit the positioning before building the paid product.

---

### Priority 5: Manually Curate 200 High-Impact Events (Days 21–75)

**What:** Research, verify (2+ Tier 1-3 sources), summarize (200 words), and tag 200 priority historical events covering 1929–2024. Focus on events in the highest-demand categories: major crashes, central bank pivots, commodity shocks, geopolitical crises, and credit events.

**Why manually:** The automated pipeline doesn't exist yet. Manual curation is how you learn what "good" looks like for an event annotation before you try to automate it. The 200 events you curate manually are the training set for everything that follows.

**Who does this:** This is 200 hours of research work, minimum. If the founding team can't absorb this, hire 2–3 research contractors (finance students or junior analysts at $20–30/hour). Budget: $4,000–6,000 in contractor costs. This is the data work that determines whether the knowledge graph is good or mediocre.

**What success looks like:** 200 events in the database, each with: title, date, 200-word summary, 2+ verified sources (Tier 1-3), market reactions (+/-1d/5d/30d for S&P and relevant assets), macro regime at time of event, and at least 2 causal edges connecting it to related events.

---

### Priority 6: Begin Angel/Pre-Seed Conversations (Days 45–90)

**What:** Identify 10–15 angel investors or pre-seed funds aligned with the RELICK thesis. Specifically: former Wall Street analysts turned investors, fintech founders, finance-adjacent angels, and pre-seed scouts at First Round, a16z, and Sequoia. Start conversations before you have a polished deck — early conversations are research, not pitches.

**Why now:** Even if the formal pre-seed raise is Month 5–6, starting conversations now accomplishes: 1) surfaces objections early while you still have time to adjust, 2) builds relationships before you need them, 3) creates a warm funnel so the actual raise is faster.

**What success looks like:** 3–5 angels who have seen the prototype and said "I'm interested when you're ready to raise." These people become your seed investors or your introduction to seed investors.

---

### Priority 7: Dead Reckoning Score — Design and Schedule (Days 30–60)

**What:** Design the Dead Reckoning Score methodology, validate it against 10 historical dates (backtesting: what would the score have been in October 2008? March 2020? January 2022?), and commit to a weekly publication schedule starting at product launch.

**Why now:** The methodology design is a thinking exercise, not an engineering exercise. It requires financial judgment, not code. Do it now while the team is small and has the bandwidth to think carefully. The publication schedule needs to start at launch — not 6 months later — for the brand-building value to compound.

**What success looks like:** A written specification of the score methodology (inputs, weighting, output format) that two people on the team can explain to a journalist in 60 seconds. A backtested output for 5 historical dates that passes the "does this feel right?" test from someone who lived through those periods.

---

### Priority 8: Identify and Lock In 2–3 Advisors (Days 21–60)

**What:** Approach 2–3 specific people for formal advisor roles with standard advisor equity grants (0.1–0.25% over 2-year vest). Target profile: one former Bloomberg/LSEG/S&P product or data executive (data moat credibility), one quantitative researcher with hedge fund background (parallel matching validation), one venture-backed fintech founder (fundraising introductions).

**Why now:** Advisors with the right profiles validate the thesis to investors in ways the founding team cannot self-validate. A former Bloomberg data executive saying "this knowledge graph approach is defensible" is worth 10 pitch meetings. Advisors also provide warm introductions to investors, early hires, and data partners.

**What success looks like:** 2 signed advisor agreements with people who will actively introduce RELICK to at least 3 investors or potential customers each.

---

### Priority 9: Calibrate the Credibility Scoring System Against Ground Truth (Days 45–75)

**What:** Build a 30-event validation dataset of historical events with known-correct credibility ratings (agreed upon by at least 2 financial domain experts). Run the credibility scoring formula against this dataset and measure: are the scores consistent with expert judgment? Where does the formula fail? Adjust the weights and the floor (currently 0.6) based on what you find.

**Why this matters:** The credibility scoring system is RELICK's primary trust mechanism. Users who trust the scores stay. Users who find errors leave and don't come back. Getting the calibration right before thousands of events are scored incorrectly is dramatically cheaper than fixing it after.

**What success looks like:** The credibility formula produces scores within 0.10 of expert consensus on >85% of the validation set. Weights adjusted based on failures. The 0.6 floor either confirmed or revised with reasoning.

---

### Priority 10: Write the Financial Model in Three Scenarios (Days 60–90)

**What:** Replace the single-scenario financial projections in BUSINESS_PLAN.md with three scenarios: Conservative (150 Analyst subs, 30 Oracle subs by EOY 1), Base (400 Analyst, 90 Oracle), and Optimistic (800 Analyst, 180 Oracle). For each scenario: model burn rate at $55K/month Year 1 (not $35K), launch CAC at $80 blended (not $25–40), and identify the exact month each scenario runs out of capital.

**Why now:** You cannot plan a fundraise without knowing how much you actually need, which requires knowing how much you actually spend (not the optimistic version), and how long it takes to get to the revenue that reduces your dependence on the raise. The current projections are too optimistic to use for internal planning. Fix them before the raise.

**What success looks like:** Three scenario models in a spreadsheet that a co-founder or investor can audit. Each model shows: monthly burn, cumulative spend, subscriber ramp by tier, MRR, and month-of-capital-exhaustion. The team agrees on which scenario they're actually planning for (recommend: Conservative as internal plan, Base as investor-facing case).

---

> *These 10 priorities are not a full roadmap. They are the foundation. If completed by Day 90, RELICK will have: a validated core product, a building waitlist, a legal foundation, a calibrated data system, the beginning of a fundraising funnel, and financial projections grounded in reality. From that position, the seed raise and full product build can proceed with confidence.*

> *If any of the top 3 priorities — prototype, user interviews, legal — reveal that the core thesis needs adjustment, you will know it in 8 weeks, not 18 months. That is the best possible outcome of this 90-day plan.*
