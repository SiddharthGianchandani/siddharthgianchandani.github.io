# Stock Market Agent

## Concept

Stock Market Agent is a research assistant for tracking market signals, company
news, watchlists, earnings dates, price moves, and risk narratives. It should
summarize what changed, why it may matter, and what evidence supports the claim.

This project should be framed as educational research software, not financial
advice.

## Portfolio Pitch

This project shows how to build an agent that works with live data, source
attribution, uncertainty, time-sensitive information, and risk-aware summaries.

## Target Users

- Students learning financial markets.
- Retail investors who want organized research notes.
- Builders exploring agentic workflows over live data.

## MVP Scope

- User creates a watchlist of tickers.
- Agent fetches recent price movement and basic company metadata.
- Agent summarizes recent news with links to sources.
- Agent creates a daily market brief for the watchlist.
- Agent flags unusual moves and explains possible causes.
- Every generated summary includes a timestamp and source links.

## Stretch Features

- Earnings calendar and post-earnings summaries.
- SEC filing summaries for 10-K, 10-Q, and 8-K reports.
- Sentiment tracking across news.
- Portfolio risk view with sector exposure.
- Alert rules for price, volume, news, or volatility changes.
- Backtesting simple signal ideas.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript.
- Backend: FastAPI or Node.js.
- Data APIs: Alpha Vantage, Polygon.io, Finnhub, Financial Modeling Prep, IEX Cloud, or Yahoo Finance wrappers.
- News: NewsAPI, Finnhub news, GDELT, RSS feeds, or curated finance sources.
- Storage: Postgres or SQLite for watchlists, briefs, and source metadata.
- Scheduling: GitHub Actions, cron, Celery, or serverless scheduled functions.

## Agent Tools

- `get_quote`: latest price, daily change, volume, market cap.
- `get_company_profile`: sector, industry, description, exchange.
- `get_recent_news`: recent articles with source URLs and timestamps.
- `summarize_filings`: parse and summarize SEC filings.
- `build_market_brief`: create a sourced daily note.
- `risk_check`: add caveats and identify missing context.

## Architecture

1. User creates a watchlist.
2. Scheduler fetches market and news data.
3. Data is normalized and stored with timestamps.
4. Agent summarizes changes and cites sources.
5. Risk checker reviews language for overconfidence.
6. Frontend displays briefs, alerts, and source trails.

## Guardrails

- Do not generate buy, sell, or hold instructions.
- Use language like "may indicate" and "possible explanation" when evidence is incomplete.
- Always include timestamps because market data changes quickly.
- Link every factual claim to a source or dataset.
- Add a clear educational-use disclaimer.

## Milestones

1. Static watchlist UI with sample data.
2. Real quote API integration.
3. News summarization with source links.
4. Daily brief generation.
5. Alert rules.
6. SEC filing summaries.
7. Evaluation set comparing generated briefs against source articles.

## Evaluation

- Are price and news summaries current and timestamped?
- Are claims traceable to sources?
- Does the agent avoid investment advice?
- Can a user understand what changed without opening five tabs?

## Website Card Copy

Stock Market Agent is an educational market research assistant that tracks
watchlists, explains unusual moves, summarizes sourced news, and produces
risk-aware daily briefs.
