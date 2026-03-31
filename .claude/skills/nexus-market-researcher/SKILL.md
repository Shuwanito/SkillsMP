---
name: nexus-market-researcher
description: "Strategic market research agent that performs competitive analysis, market sizing (TAM/SAM/SOM), pricing intelligence, opportunity scoring, and go-to-market strategy development. Covers B2B SaaS, EdTech, HealthTech, LegalTech, and HR Tech verticals with real-time web research."
license: proprietary
compatibility: "Any LLM agent with web search and filesystem access. Outputs JSON or Markdown reports. Uses live web search for current market data -- no static database dependency."
metadata:
  department: strategy
  agents: ["cso", "strategy-partnerships"]
  price_per_execution: "$1.50"
  version: "2.0.0"
  publishable: true
  categories: ["market-research", "competitive-analysis", "pricing", "strategy", "go-to-market"]
  trigger_keywords: ["market research", "competitive analysis", "market size", "TAM SAM SOM", "pricing strategy", "go-to-market", "competitor analysis", "market opportunity", "industry analysis", "business intelligence"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Market Researcher

Real-time strategic market research that delivers structured competitive intelligence, validated market sizing, and actionable go-to-market strategies -- not generic industry overviews.

## When to Use This Skill

- **Market sizing**: Calculate TAM/SAM/SOM for a product or market segment
- **Competitive analysis**: Deep-dive on specific competitors or landscape mapping
- **Pricing intelligence**: Analyze competitor pricing, identify positioning opportunities
- **Opportunity scoring**: Evaluate and rank market entry opportunities
- **Go-to-market strategy**: Develop launch plans for new products or market segments
- **Partnership/M&A scouting**: Identify and evaluate potential partners or acquisition targets
- **Trend monitoring**: Track emerging technologies, regulations, or market shifts

## Instructions

### Step 1: Define Research Scope

Determine what the user needs:

| Research Type | Required Input | Output |
|--------------|----------------|--------|
| Market Sizing | Product/service description + target geography | TAM/SAM/SOM with methodology |
| Competitive Analysis | Competitor names OR market segment | Feature matrix + positioning map |
| Pricing Intelligence | Product category + competitor list | Price comparison + positioning recommendation |
| Opportunity Scoring | List of potential markets/segments | Ranked opportunities with scores |
| Go-to-Market | Product + target market + resources | Phased launch plan |
| Partnership Scouting | Strategic goals + criteria | Ranked list of candidates with fit scores |

### Step 2: Execute Research

**For Market Sizing (TAM/SAM/SOM):**

Use both top-down and bottom-up approaches, then reconcile:

*Top-down:*
1. Find total industry revenue from analyst reports (Gartner, IDC, Grand View Research, Mordor Intelligence)
2. Apply geographic filter (% of global market for target region)
3. Apply segment filter (% of industry that matches your product category)

*Bottom-up:*
1. Estimate number of potential customers in target segment
2. Multiply by average annual contract value (ACV) or revenue per customer
3. Apply realistic penetration rates (typically 1-5% for year 1)

*Output format:*
```json
{
  "market_sizing": {
    "product": "AI-powered code review tool",
    "geography": "North America + EU",
    "methodology": "Top-down from DevOps market reports + bottom-up from developer population",
    "TAM": {
      "value": "$4.2B",
      "definition": "Total developer tools market for code quality (NA + EU)",
      "sources": ["Gartner DevOps Market Guide 2025", "IDC Developer Tools Forecast"]
    },
    "SAM": {
      "value": "$680M",
      "definition": "AI-assisted code review segment, teams >10 developers",
      "calculation": "16.2% of TAM -- AI code review adoption rate in enterprise"
    },
    "SOM": {
      "value": "$12M",
      "definition": "Realistic Year 1 capture: SMB + mid-market, self-serve + sales-assisted",
      "calculation": "~1.8% of SAM -- based on comparable SaaS launch benchmarks",
      "assumptions": ["$200/seat/year average", "60,000 seats in Year 1", "15-month sales cycle for enterprise"]
    },
    "growth_rate": "23.4% CAGR 2025-2030",
    "confidence": 0.72,
    "data_freshness": "Sources from Q4 2025 - Q1 2026"
  }
}
```

**For Competitive Analysis:**

For each competitor, research and structure:

1. **Company overview**: Founded, funding, revenue (estimated if private), headcount, growth trajectory
2. **Product**: Core features, unique differentiators, known limitations, tech stack (if public)
3. **Pricing**: Tiers, per-seat vs per-usage, free tier limitations, enterprise pricing
4. **Market position**: Target segment (SMB/mid-market/enterprise), geographic focus, industry verticals
5. **GTM strategy**: Sales model (PLG, sales-led, hybrid), key channels, partnership strategy
6. **Strengths**: What they do well, moat/defensibility
7. **Weaknesses**: Known gaps, customer complaints (from G2, Capterra, Reddit)
8. **Threat level**: LOW / MEDIUM / HIGH based on overlap with your product

Build a feature comparison matrix:

| Feature | Your Product | Competitor A | Competitor B | Competitor C |
|---------|-------------|-------------|-------------|-------------|
| Feature 1 | Full | Partial | Full | None |
| Feature 2 | Full | Full | None | Full |
| Pricing (per seat/mo) | $20 | $35 | $15 | $25 |

Identify positioning whitespace -- combinations of features/price/segment that no competitor fully occupies.

**For Pricing Intelligence:**

1. Collect pricing data for 5-10 competitors (public pricing pages + G2/Capterra data)
2. Map pricing models: per-seat, per-usage, flat rate, freemium, outcome-based
3. Identify pricing tiers and what gates each tier (features, usage limits, support level)
4. Calculate price-per-value metrics (cost per user, cost per feature set)
5. Recommend positioning:
   - **Premium**: 20-40% above market average (requires clear differentiation)
   - **Market rate**: Within 10% of median (compete on features/experience)
   - **Penetration**: 20-40% below average (capture share, upgrade later)
   - **Outcome-based**: Per-result pricing (premium positioning, higher margins)

**For Opportunity Scoring:**

Score each opportunity on 5 dimensions (1-10 each):

| Dimension | Weight | What It Measures |
|-----------|--------|-----------------|
| Market size | 25% | TAM and growth rate |
| Competition | 20% | Number and strength of incumbents |
| Fit | 25% | Alignment with existing capabilities |
| Urgency | 15% | Buyer pain level and willingness to pay |
| Feasibility | 15% | Time and resources to enter |

Final score = weighted average. Rank all opportunities and recommend top 3.

### Step 3: Validate and Cross-Reference

- Every market size claim needs at least 2 sources
- Competitor information validated against their website + third-party reviews
- Revenue estimates for private companies use employee count heuristics ($150K-250K revenue/employee for SaaS)
- Flag data that is older than 12 months as potentially outdated
- Note confidence level for each major claim

### Step 4: Generate Actionable Report

Every report must end with:
1. **Key findings** (3-5 bullet points)
2. **Strategic recommendations** (prioritized, with rationale)
3. **Risks and uncertainties** (what could invalidate the analysis)
4. **Suggested next steps** (specific actions, not vague advice)

## Example Input/Output

**Input**: "Analyze the competitive landscape for AI-powered legal document review tools in Europe. We're considering entering this market."

**Output** (abbreviated):
- **Market size**: EUR 1.8B TAM (European LegalTech), EUR 340M SAM (AI document review)
- **Key competitors**: Luminance (UK, $100M+ ARR, enterprise-focused), Kira Systems (Litera), ContractPodAI, Legartis (Swiss, DACH focus)
- **Positioning whitespace**: No strong player in SMB segment (<50 employees) with EU-language support beyond English/German
- **Pricing landscape**: Enterprise contracts EUR 50K-500K/year; SMB opportunity at EUR 200-500/seat/month
- **Recommendation**: Enter via SMB segment with multi-language EU support (Spanish, French, Italian, Portuguese). Differentiate on price (40% below enterprise tools) and ease of setup. Avoid direct competition with Luminance in enterprise.
- **Risk**: EU AI Act high-risk classification for legal AI may increase compliance costs 20-30%

## Edge Cases

- **Emerging market with no analyst reports**: Use bottom-up sizing only, flag lower confidence
- **No public pricing for competitors**: Use job postings (sales quotas hint at ACV), customer reviews, and employee count heuristics
- **Highly fragmented market**: Focus on top 10 by market share, group rest as "long tail"
- **Single-country analysis**: Note regulatory and cultural factors that limit applicability to other markets
- **Rapidly changing market** (e.g., AI): Flag that analysis has a 3-6 month shelf life and recommend refresh
- **B2B2C models**: Size both the direct buyer market and the end-user market separately

## What Sets This Apart

- **Real-time web research** -- not pre-trained knowledge. Every execution searches current sources.
- **Dual methodology** market sizing (top-down + bottom-up) with reconciliation
- **Competitive feature matrices** with actual pricing data, not just company descriptions
- **Opportunity scoring framework** that makes market entry decisions data-driven
- Identifies **positioning whitespace** -- where to compete, not just who to compete with
- Every claim **sourced and confidence-rated** -- no hallucinated market numbers

## Pricing

- Per-execution: $1.50 (single research query or competitive profile)
- Full market analysis (sizing + competitive + pricing + GTM): $6.00
- Volume: 20% discount at 30+ research queries/month
- Enterprise: Monthly retainer for continuous market monitoring

## Output Format

Default: JSON. Supports `format: "markdown"` for presentation-ready reports, `format: "slides"` for executive summary bullet points.
