# Signal Atlas

## Concept

Signal Atlas is an interactive data exploration dashboard that helps users find
meaningful patterns in noisy datasets. The product should feel like a map of
signals: trends, anomalies, correlations, clusters, and surprising outliers.

## Portfolio Pitch

Build a visual system that turns raw data into a clear story. This project can
show data cleaning, exploratory analysis, dashboard design, and product thinking.

## Target Users

- Students exploring public datasets.
- Analysts who need a clean first pass on unfamiliar data.
- Builders who want a visual way to explain patterns.

## MVP Scope

- Upload or select a sample dataset.
- Show dataset overview: rows, columns, missingness, data types, distributions.
- Generate charts for numeric, categorical, and time-series columns.
- Detect basic signals: outliers, correlations, missing-data hotspots, trend shifts.
- Let users save a small "insight board" with chart snapshots and notes.

## Stretch Features

- Natural-language questions over the dataset.
- Auto-generated insight summaries.
- Dataset comparison mode.
- Exportable report as PDF or Markdown.
- Public gallery of example analyses.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript, Tailwind CSS or CSS modules.
- Charts: Observable Plot, Vega-Lite, ECharts, or Plotly.
- Data processing: Arquero, DuckDB-WASM, or a Python FastAPI backend.
- Optional backend: FastAPI with Pandas and Polars.
- Storage: local browser storage for MVP, SQLite or Postgres later.

## Data Sources

- Kaggle public datasets.
- Data.gov.
- World Bank open data.
- NYC Open Data.
- Personal CSV examples created for demos.

## Architecture

1. Dataset loader imports CSV or sample data.
2. Data profiler calculates column metadata and quality metrics.
3. Signal engine detects outliers, correlations, missingness, and trends.
4. Visualization layer renders chart recommendations.
5. Insight board stores selected charts and user notes.

## Milestones

1. Static dashboard with one sample dataset.
2. CSV upload and automatic profiling.
3. Signal detection engine.
4. Insight board.
5. Portfolio case study with screenshots and lessons learned.

## Evaluation

- Can a new user understand a dataset in under two minutes?
- Are the generated charts useful without manual configuration?
- Do detected signals match what a human analyst would notice?

## Website Card Copy

Signal Atlas is a data storytelling dashboard that scans messy datasets for
trends, outliers, correlations, and surprising signals, then helps turn them into
clear visual narratives.
