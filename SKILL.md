---
name: comp-analysis
description: Cross-company comparative analysis with sector benchmarking
---

# Competitive Analysis Skill

Perform a structured, multi-stage competitive analysis across two or more publicly traded companies. This skill gathers financial data from SEC filings and market sources, normalizes metrics for fair comparison, runs a head-to-head analysis across key dimensions, and produces a comprehensive report with comparison tables, rankings, and sector positioning.

## When to Use This Skill

Use this skill when you need to:

- **Compare companies within the same sector** -- e.g., evaluate AAPL vs. MSFT vs. GOOGL in the technology sector.
- **Benchmark a company against its peers** -- understand where a company stands relative to competitors on growth, profitability, valuation, and balance sheet strength.
- **Evaluate investment alternatives** -- identify which company in a peer group is the strongest candidate based on quantitative metrics and qualitative positioning.
- **Prepare sector-level research** -- build a structured view of how multiple players in an industry compare across standardized financial dimensions.
- **Support portfolio construction** -- determine relative allocation weights by understanding competitive dynamics within a sector.

This skill is NOT intended for single-company deep dives (use the deep-dive skill instead) or for earnings-specific analysis (use the earnings-review skill instead).

## Methodology

The analysis proceeds through four sequential stages, each building on the output of the previous one.

### Stage 1: Gather

**Model:** Claude Haiku 3.5 | **Temperature:** 0 | **Tools:** edgar, market-data

Collect raw financial data for every company in the input ticker list. For each company, the gather stage pulls:

- Latest annual filing (10-K) and most recent quarterly filing (10-Q) from SEC EDGAR
- Current market data including stock price, market capitalization, and trading volume
- Valuation metrics such as P/E ratio, EV/EBITDA, price-to-book, and price-to-sales
- Key income statement, balance sheet, and cash flow figures

If a sector is provided, it is used as context to guide which metrics are most relevant for the comparison (e.g., SaaS metrics for software companies, same-store sales for retail).

**Output format:** JSON -- structured data for each ticker with all gathered metrics.

### Stage 2: Normalize

**Model:** Claude Haiku 3.5 | **Temperature:** 0

Normalize the gathered data across all companies so that comparisons are fair and meaningful. This stage addresses common challenges in cross-company comparison:

- **Fiscal year alignment** -- companies may have different fiscal year end dates; metrics are adjusted to comparable periods.
- **Reporting standard differences** -- adjustments for GAAP vs. non-GAAP reporting where applicable.
- **Scale normalization** -- converting absolute figures to ratios and percentages for fair comparison across companies of different sizes.
- **Metric standardization** -- ensuring consistent calculation methods for:
  - Revenue growth (YoY and QoQ)
  - Gross margin, operating margin, net margin
  - P/E ratio (trailing and forward)
  - EV/EBITDA
  - Debt-to-equity ratio
  - Return on equity (ROE)
  - Free cash flow yield

**Output format:** JSON -- normalized metrics for all companies in a consistent, comparable structure.

### Stage 3: Compare

**Model:** Claude Sonnet 4 | **Temperature:** 0.3

Perform a detailed head-to-head comparative analysis using the normalized data. Companies are ranked across five key dimensions:

1. **Growth** -- revenue growth trajectory, earnings growth, market expansion potential.
2. **Profitability** -- margin profile, earnings quality, operating leverage.
3. **Valuation** -- relative cheapness or expensiveness vs. peers on multiple valuation metrics.
4. **Balance sheet strength** -- leverage, liquidity, debt maturity profile, cash position.
5. **Market position** -- competitive moat, market share trends, strategic positioning.

For each dimension, every company receives a relative ranking with supporting evidence. Strengths and weaknesses are identified for each company in context of the peer group.

**Output format:** Markdown -- structured comparative analysis with per-dimension rankings.

### Stage 4: Report

**Model:** Claude Sonnet 4 | **Temperature:** 0.4

Generate the final comprehensive competitive analysis report. This stage synthesizes the comparison into a polished, actionable document that includes:

- **Executive summary** -- high-level findings and which companies stand out.
- **Comparison tables** -- side-by-side metrics across all companies.
- **Relative rankings** -- ordered ranking across each dimension with visual indicators.
- **Sector positioning analysis** -- how each company fits within the broader sector landscape.
- **Key differentiators** -- what makes each company unique relative to peers.
- **Potential catalysts** -- upcoming events or trends that could shift relative positioning.
- **Risk factors** -- company-specific and sector-wide risks to monitor.

**Output format:** Markdown -- the primary output of the pipeline.

## Output Format

The final report is delivered in Markdown format and includes:

- Comparison tables with normalized metrics for all companies
- Per-dimension rankings (growth, profitability, valuation, balance sheet, market position)
- Sector positioning narrative
- Key differentiators and catalysts for each company
- Summary conclusions with actionable takeaways

Intermediate outputs (gather, normalize, compare) are also saved for reference and auditability.

The output file is named using the pattern `comp-analysis-{sector}` where `{sector}` is the provided sector input.

## Notes on Fair Comparison

- **Peer selection matters.** The quality of the analysis depends heavily on comparing genuinely comparable companies. Comparing a mega-cap tech company against a small-cap biotech will produce technically valid but analytically less useful results.
- **Sector context improves relevance.** Providing the `sector` input allows the pipeline to weight industry-specific metrics more heavily (e.g., subscriber growth for streaming, NRR for SaaS, same-store sales for retail).
- **Fiscal year timing.** Companies with different fiscal year end dates may report data from different calendar periods. The normalization stage attempts to account for this, but significant timing gaps should be noted when interpreting results.
- **Data availability.** Not all metrics are available for all companies. The analysis works with available data and notes where gaps exist rather than fabricating estimates.
- **Point-in-time snapshot.** This analysis reflects data available at the time of execution. Market conditions, stock prices, and financial metrics change continuously.
