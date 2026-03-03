# @foundationalresearch/companalysis

[![npm version](https://img.shields.io/npm/v/@foundationalresearch/companalysis)](https://www.npmjs.com/package/@foundationalresearch/companalysis)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![AI Skill](https://img.shields.io/badge/AI%20Skill-Pipeline-blueviolet)](https://github.com/FoundationalResearch/companalysis)

**Standalone AI skill for competitive company analysis.** Compare multiple publicly traded companies across growth, profitability, valuation, balance sheet strength, and market position -- all through a structured 4-stage pipeline.

This package contains no executable code. It is a declarative skill definition (YAML pipeline + Markdown skill document) designed to be consumed by AI coding agents and financial analysis tools such as [Scrutari](https://github.com/scrutari/scrutari), Claude Code, Cursor, and Codex.

---

## Install

```bash
npx skills add FoundationalResearch/skills@comp-analysis
```

Works with **Claude Code, Cursor, GitHub Copilot, Codex, Gemini, Windsurf**, and [20+ other agents](https://skills.sh).

> Part of the [FoundationalResearch Skills](https://github.com/FoundationalResearch/skills) collection -- 14 professional financial analysis skills for AI agents.

---

## What It Does

Given a list of ticker symbols and an optional sector, this skill runs a four-stage analysis pipeline:

```
Gather --> Normalize --> Compare --> Report
```

1. **Gather** -- Pulls SEC filings (10-K, 10-Q), current market data, and valuation metrics for every company in the list.
2. **Normalize** -- Standardizes all metrics for fair cross-company comparison, accounting for fiscal year timing, reporting standards, and scale differences.
3. **Compare** -- Ranks companies head-to-head across five dimensions: growth, profitability, valuation, balance sheet strength, and market position.
4. **Report** -- Produces a comprehensive competitive analysis report with comparison tables, rankings, sector positioning, and actionable takeaways.

## What You Get

The final output is a structured Markdown report containing:

- **Comparison tables** -- side-by-side normalized metrics for all companies
- **Dimensional rankings** -- ordered rankings across growth, profitability, valuation, balance sheet, and market position
- **Sector positioning analysis** -- how each company fits within the broader industry landscape
- **Key differentiators** -- what makes each company unique relative to its peers
- **Potential catalysts** -- upcoming events or trends that could shift competitive dynamics
- **Risk assessment** -- company-specific and sector-wide risks

All intermediate stage outputs (raw data, normalized metrics, comparative analysis) are also saved for transparency and auditability.

## Also available on npm

```bash
npm install @foundationalresearch/companalysis
```

The package installs the following files into your `node_modules`:

| File | Purpose |
|------|---------|
| `pipeline.yaml` | Pipeline definition with 4 stages, model configs, and prompts |
| `SKILL.md` | Agent-readable skill document with methodology and usage guidance |
| `README.md` | This file |
| `LICENSE` | MIT license |

## Pipeline Configuration

### Inputs

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `tickers` | `string[]` | Yes | List of ticker symbols to compare (e.g., `["AAPL", "MSFT", "GOOGL"]`) |
| `sector` | `string` | No | Industry sector for contextual benchmarks (e.g., `"Technology"`) |

### Tool Dependencies

| Tool | Required | Purpose |
|------|----------|---------|
| `edgar` | Yes | SEC EDGAR API for filings and financial data |
| `market-data` | Yes | Stock quotes, OHLC, and valuation metrics |
| `news` | No | Recent news articles for additional context |

### Stage Details

| Stage | Model | Temp | Output | Description |
|-------|-------|------|--------|-------------|
| `gather` | Claude Haiku 3.5 | 0 | JSON | Collect filings, market data, and valuations for all tickers |
| `normalize` | Claude Haiku 3.5 | 0 | JSON | Standardize metrics for fair cross-company comparison |
| `compare` | Claude Sonnet 4 | 0.3 | Markdown | Head-to-head ranking across 5 dimensions |
| `report` | Claude Sonnet 4 | 0.4 | Markdown | Final report with tables, rankings, and analysis |

### Pipeline DAG

```
gather
  |
  v
normalize
  |
  v
compare
  |
  v
report
```

Stages execute sequentially. Each stage receives the output of the previous stage as input. The `gather` stage uses tools (edgar, market-data) to fetch live data; subsequent stages are pure LLM analysis.

## Usage

### With Claude Code

Place the `SKILL.md` file in your project or reference it via the installed package. Claude Code will discover the skill and can invoke the pipeline when asked to compare companies:

```
> Compare AAPL, MSFT, and GOOGL in the technology sector
```

### With Cursor

Add the `SKILL.md` to your project context or rules. Cursor's agent will use the methodology described in the skill document to structure its competitive analysis.

### With Codex

Reference `SKILL.md` as a context document. Codex will follow the defined stages and methodology when performing comparative analysis tasks.

### With Scrutari

Scrutari natively supports pipeline skills. The `pipeline.yaml` is loaded automatically:

```
> /run comp-analysis --tickers AAPL,MSFT,GOOGL --sector Technology
```

Or through natural language:

```
> Compare Apple, Microsoft, and Google as technology investments
```

## Example

Comparing cloud infrastructure providers:

```
Inputs:
  tickers: ["AMZN", "MSFT", "GOOGL"]
  sector: "Cloud Infrastructure"

Output: comp-analysis-Cloud Infrastructure.md
```

The report would include:
- Revenue and growth comparison across AWS, Azure, and GCP
- Margin analysis (cloud segment where available, consolidated otherwise)
- Valuation multiples relative to growth rates
- Balance sheet comparison (cash, debt, capex intensity)
- Market share trends and competitive positioning
- Catalysts: AI infrastructure spending, enterprise migration trends
- Risks: antitrust, margin compression, capital intensity

## File Structure

```
companalysis/
  package.json       Package metadata and npm configuration
  pipeline.yaml      4-stage pipeline definition
  SKILL.md           Agent-readable skill with methodology
  README.md          This documentation
  LICENSE            MIT license
```

## Related Skills

| Skill | Package | Description |
|-------|---------|-------------|
| Deep Dive | `@foundationalresearch/deepdive` | Comprehensive single-company analysis (6 stages) |
| Earnings Review | `@foundationalresearch/earningsreview` | Earnings-focused analysis with guidance tracking |
| Thesis Generation | -- | Investment thesis construction |

## Contributing

Issues and pull requests are welcome at [github.com/FoundationalResearch/companalysis](https://github.com/FoundationalResearch/companalysis).

## License

MIT -- see [LICENSE](./LICENSE) for details.
