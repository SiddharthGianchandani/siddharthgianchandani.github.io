# Multi-Agent Stock Market Research System

## Concept

Multi-Agent Stock Market Research System is an educational research assistant
that analyzes a stock through multiple investment lenses. One orchestrator agent
collects data, delegates analysis to specialist subagents, resolves conflicts,
and produces a sourced summary for the user.

This project should be framed as investment research software, not financial
advice. It should not issue buy, sell, or hold instructions.

## Portfolio Pitch

This project demonstrates multi-agent architecture, live financial data
pipelines, source-grounded summarization, disagreement synthesis, risk-aware
language, and time-sensitive research workflows.

## Target Users

- Students learning financial markets.
- Retail investors who want organized research notes.
- Builders exploring multi-agent workflows over live data.

## Agent Architecture

### Orchestrator Agent

Responsibilities:

- Accept ticker, company name, time horizon, and user research goal.
- Build one request-scoped research packet with quotes, financials, news,
  filings, five-year price history, sector data, and timestamps.
- Dispatch the same packet to all subagents.
- Enforce output schema for every subagent.
- Detect agreement, disagreement, missing data, and overconfident claims.
- Summarize the investment committee view for the user.
- Include source links and data timestamps.
- Avoid buy/sell/hold recommendations.

### Strategy Subagents

Each strategy subagent should answer from its own investment philosophy. It
should not try to be the whole analyst.

1. `value_agent`
   - Focus: valuation relative to fundamentals.
   - Inputs: P/E, forward P/E, P/B, EV/EBITDA, free cash flow yield, revenue,
     margins, debt, peer comparisons.
   - Output question: "Does the stock appear expensive or cheap relative to its
     fundamentals and peers?"

2. `growth_agent`
   - Focus: revenue growth, earnings growth, guidance, market opportunity, and
     reinvestment.
   - Inputs: revenue CAGR, EPS growth, analyst estimates, guidance, product
     momentum, addressable market notes.
   - Output question: "Is the company growing fast enough to support its
     current valuation?"

3. `quality_agent`
   - Focus: durable financial strength.
   - Inputs: ROIC, ROE, gross margin, operating margin, free cash flow,
     earnings consistency, debt ratios.
   - Output question: "Does this look like a high-quality business?"

4. `momentum_agent`
   - Focus: price trend and market behavior.
   - Inputs: moving averages, relative strength, 52-week range, volume, trend
     breaks, recent returns.
   - Output question: "Is market momentum currently favorable or unfavorable?"

5. `dividend_income_agent`
   - Focus: income and dividend sustainability.
   - Inputs: dividend yield, payout ratio, dividend growth, free cash flow
     coverage, debt, dividend history.
   - Output question: "Is this a sustainable income candidate?"

6. `low_volatility_defensive_agent`
   - Focus: downside resilience and defensive characteristics.
   - Inputs: beta, volatility, drawdowns, sector cyclicality, revenue stability,
     recession sensitivity.
   - Output question: "Could this stock hold up better in difficult markets?"

7. `event_driven_agent`
   - Focus: recent catalysts.
   - Inputs: earnings, guidance changes, product launches, M&A, analyst
     revisions, regulatory events, lawsuits, management changes, major news.
   - Output question: "What recent events may change the investment thesis?"

8. `macro_sector_agent`
   - Focus: macroeconomic and sector context.
   - Inputs: interest rates, inflation, currency, commodities, sector
     performance, industry cycle, regulatory environment.
   - Output question: "Is the broader environment supportive or hostile?"

9. `risk_agent`
   - Focus: what could go wrong.
   - Inputs: volatility, concentration, liquidity, leverage, valuation risk,
     business risk, regulatory risk, earnings risk, news risk.
   - Output question: "What are the biggest risks and uncertainty points?"

### Research Subagents

10. `company_stability_research_agent`
    - Focus: determine how stable the company appears as a business.
    - Inputs: revenue consistency, profitability consistency, cash flow,
      leverage, liquidity, credit ratings if available, customer concentration
      hints, competitive position, management continuity, filing risk factors.
    - Output question: "How stable is this company operationally and
      financially?"

11. `five_year_price_fluctuation_agent`
    - Focus: analyze how the stock price fluctuated over the past five years.
    - Inputs: five-year daily or weekly price history, annual returns,
      drawdowns, volatility, major peaks/troughs, split-adjusted prices, market
      index comparison.
    - Output question: "How volatile has this stock been, and what major price
      regimes or drawdowns appeared over five years?"

## Subagent Output Schema

Each subagent should return:

```json
{
  "agent_name": "value_agent",
  "stance": "positive | neutral | negative | mixed | insufficient_data",
  "confidence": "low | medium | high",
  "thesis": "one paragraph summary",
  "supporting_points": [
    {
      "claim": "specific claim",
      "evidence": "metric, filing, news item, or price statistic",
      "source_id": "source reference from research packet"
    }
  ],
  "concerns": ["risk or caveat"],
  "metrics_used": ["P/E", "FCF yield"],
  "missing_data": ["metric or source not available"]
}
```

## Orchestrator Output

The orchestrator should return:

- `executive_summary`: concise educational research summary.
- `committee_view`: where agents agree and disagree.
- `bull_case`: strongest positive points.
- `bear_case`: strongest negative points.
- `strategy_breakdown`: one short section per subagent.
- `company_stability`: summary from stability research agent.
- `five_year_price_behavior`: summary from price fluctuation agent.
- `key_risks`: consolidated risk list.
- `what_to_monitor_next`: earnings, metrics, news, macro indicators, or filings.
- `sources`: source links and timestamps.
- `disclaimer`: educational-use and not-financial-advice statement.

## Request-Scoped Research Packet

The orchestrator creates one immutable research packet per ticker request.

Packet contents:

- ticker and company metadata.
- latest quote and timestamp.
- five-year adjusted price history.
- market index comparison, such as S&P 500 or relevant sector ETF.
- financial statements and key ratios.
- recent earnings and guidance.
- recent company news.
- SEC filings and risk factors when available.
- analyst estimates when available.
- sector and macro context.
- source IDs for every data item.

Subagents must use only this packet unless the orchestrator explicitly performs
an additional sourced retrieval for missing information.

## MVP Scope

- User enters one ticker.
- System builds a research packet from live or cached data.
- Run five subagents first in code: value, growth, momentum, risk, and
  five-year price fluctuation.
- Stub remaining subagents with schema-compliant placeholder logic until data
  sources are ready.
- Orchestrator summarizes all available outputs.
- Store request, packet metadata, subagent outputs, and final summary.
- Include timestamps and source links.

## Full Scope

- All nine strategy subagents.
- Two research subagents.
- Watchlists.
- Daily or weekly briefs.
- Earnings and filing summaries.
- Historical research archive by ticker.
- Side-by-side comparison of multiple stocks.
- User-adjustable strategy weights.
- Backtesting simple signals for educational use.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript.
- Backend: FastAPI or Node.js.
- Agent orchestration: simple Python orchestrator first; LangGraph later if
  graph state becomes complex.
- Data APIs: Alpha Vantage, Polygon.io, Finnhub, Financial Modeling Prep,
  Tiingo, IEX Cloud, or Yahoo Finance wrappers.
- Filings: SEC EDGAR APIs.
- News: Finnhub news, GDELT, RSS feeds, NewsAPI, or curated finance sources.
- Storage: Postgres for requests, research packets, subagent outputs, and final
  summaries.
- Scheduling: GitHub Actions, cron, Celery, or serverless scheduled functions.

## Agent Tools

- `get_quote`: latest price, daily change, volume, market cap.
- `get_company_profile`: sector, industry, description, exchange.
- `get_price_history`: five-year split-adjusted price series.
- `calculate_price_statistics`: returns annual returns, volatility, drawdowns,
  moving averages, and 52-week range.
- `get_financial_ratios`: valuation, profitability, leverage, liquidity.
- `get_financial_statements`: income statement, balance sheet, cash flow.
- `get_recent_news`: recent articles with source URLs and timestamps.
- `summarize_filings`: parse and summarize SEC filings.
- `get_sector_context`: sector performance and macro indicators.
- `build_research_packet`: create request-scoped evidence bundle.
- `validate_agent_output`: enforce schema and source references.
- `risk_language_check`: block financial advice and overconfident language.

## Database Design

### `research_requests`

- `id`
- `ticker`
- `company_name`
- `requested_at`
- `user_goal`
- `status`

### `research_packets`

- `id`
- `request_id`
- `packet_json`
- `packet_hash`
- `created_at`
- `source_timestamps_json`

### `subagent_outputs`

- `id`
- `request_id`
- `packet_id`
- `agent_name`
- `stance`
- `confidence`
- `output_json`
- `created_at`

### `orchestrator_summaries`

- `id`
- `request_id`
- `packet_id`
- `summary_json`
- `disclaimer_text`
- `created_at`

## Guardrails

- Do not generate buy, sell, or hold instructions.
- Do not provide personalized financial advice.
- Use language like "may indicate" and "possible explanation" when evidence is
  incomplete.
- Always include timestamps because market data changes quickly.
- Link every factual claim to a source or dataset.
- Keep subagents grounded in the request-scoped research packet.
- Do not let subagents use prior ticker requests unless the user explicitly asks
  for comparison.
- Add a clear educational-use disclaimer.
- Ask the user to consult a qualified financial advisor for personalized
  decisions.

## Milestones

1. Static single-ticker UI with sample packet and sample subagent outputs.
2. Real quote and company profile integration.
3. Five-year price history retrieval and fluctuation analysis.
4. Value, growth, momentum, risk, and five-year price agents.
5. Orchestrator summary with disagreement synthesis.
6. Database persistence for packets, subagent outputs, and summaries.
7. Company stability research agent.
8. Remaining strategy agents: quality, dividend, defensive, event, macro.
9. Source-grounding validation for every subagent claim.
10. Watchlist brief.
11. Filing and earnings integration.
12. Evaluation set comparing summaries against source packets.

## Evaluation

- Are price and news summaries current and timestamped?
- Are claims traceable to source IDs in the packet?
- Does each subagent stay within its strategy lens?
- Does the orchestrator preserve disagreement instead of flattening it?
- Does the agent avoid investment advice?
- Does the five-year fluctuation agent compute volatility and drawdowns
  correctly?
- Does the company stability agent distinguish financial stability from stock
  price stability?
- Can a user understand the bull case, bear case, risks, and monitoring points
  without opening many tabs?

## Website Card Copy

Multi-Agent Stock Market Research System analyzes a stock through 11 specialist
agents, including value, growth, quality, momentum, income, defensive, event,
macro, risk, company stability, and five-year price fluctuation lenses. The
orchestrator synthesizes agreement, disagreement, risks, and sourced evidence
for educational research.
