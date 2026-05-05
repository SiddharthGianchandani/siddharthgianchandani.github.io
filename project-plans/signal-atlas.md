# Signal Atlas

## Concept

Signal Atlas is a hypothesis discovery engine for tabular datasets. It scans a
dataset, ranks interesting patterns, explains why each pattern may matter, and
recommends which statistical test or follow-up analysis should validate it.

The goal is not to replace Microsoft Fabric notebooks, Data Wrangler, or BI
dashboards. Those tools are strong for exploration, visualization, and enterprise
data workflows. Signal Atlas should focus on the earlier question:

"What in this dataset is worth investigating next?"

## Portfolio Pitch

Build an insight triage system that converts messy data into ranked hypotheses.
It should show analytical judgment, product thinking, visualization, and a clean
handoff into deeper statistical testing with the Sales Data Science Agent.

## Differentiation From Fabric

Fabric notebooks and Data Wrangler can display data, create visuals, and support
exploratory analysis. Signal Atlas should not compete as a smaller notebook.

Signal Atlas should be different because it:

- Is not notebook-first.
- Is designed for nontechnical or semi-technical users.
- Ranks signals by potential importance, not just chart availability.
- Explains why a pattern may be interesting.
- Suggests validation methods such as T tests, ANOVA, regression, correlation,
  chi-square, difference-in-differences, or BSTS.
- Creates a structured insight board rather than a loose notebook thread.
- Produces hypotheses that can be sent to the Sales Data Science Agent.

## Target Users

- Analysts who need fast triage before deeper analysis.
- Business users who want to know what changed, which segments differ, and what
  deserves testing.
- Students learning how exploratory data analysis turns into testable
  hypotheses.

## MVP Scope

- Upload or select a sample CSV or Excel dataset.
- Profile rows, columns, missingness, data types, duplicates, and distributions.
- Detect candidate signals:
  - outliers
  - missing-data hotspots
  - correlations
  - time shifts
  - segment differences
  - trend changes
  - unusual category concentration
- Rank signals by strength, data quality, and business relevance proxy.
- Generate a hypothesis card for each signal.
- Recommend a validation test for each hypothesis.
- Let users save selected hypotheses to an insight board.
- Export the insight board as Markdown.

## Hypothesis Card

Each detected signal should become a structured card:

```json
{
  "signal_id": "sig_001",
  "title": "Segment A has higher average sales than Segment B",
  "signal_type": "segment_difference",
  "why_it_matters": "The difference may indicate a meaningful customer segment effect.",
  "evidence": {
    "metric": "sales",
    "segment_column": "customer_segment",
    "group_a_mean": 128.4,
    "group_b_mean": 97.2,
    "difference_percent": 32.1,
    "rows_used": 1842
  },
  "confidence": "medium",
  "data_quality_warnings": ["Segment B has fewer observations"],
  "recommended_test": "two_sample_t_test",
  "handoff_payload": {
    "target_metric": "sales",
    "group_column": "customer_segment",
    "groups": ["A", "B"]
  }
}
```

## Signal Ranking

Ranking should combine:

- Effect size or pattern strength.
- Number of rows supporting the signal.
- Missing-data risk.
- Outlier sensitivity.
- Whether the signal is actionable.
- Whether a clear validation test exists.
- Whether the pattern is repeated across time or segments.

## Handoff To Sales Data Science Agent

Signal Atlas should create a `handoff_payload` that can pre-fill a statistical
test in the Sales Data Science Agent.

Examples:

- Segment difference -> T test or ANOVA.
- Before/after change -> paired T test, difference-in-differences, or BSTS.
- Numeric relationship -> correlation or regression.
- Category relationship -> chi-square test.
- Time-series shift -> time-series decomposition or BSTS.

This gives the portfolio a clean workflow:

1. Signal Atlas discovers possible patterns.
2. Sales Data Science Agent validates the strongest hypotheses.
3. Results are summarized with grounded evidence and caveats.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript.
- Charts: Observable Plot, Vega-Lite, ECharts, or Plotly.
- Data processing: DuckDB-WASM, Arquero, or Polars through a backend.
- Optional backend: FastAPI with Pandas or Polars.
- Storage: local browser storage for MVP, SQLite or Postgres later.

## Data Sources

- Synthetic sales datasets created for demos.
- Kaggle public datasets.
- Data.gov.
- World Bank open data.
- NYC Open Data.
- User-uploaded CSV or Excel files.

## Architecture

1. Dataset loader imports CSV, Excel, or sample data.
2. Data profiler calculates metadata and quality metrics.
3. Signal detectors generate candidate hypotheses.
4. Ranking engine scores and orders hypotheses.
5. Visualization layer renders evidence charts for each signal.
6. Insight board stores selected hypothesis cards.
7. Handoff builder exports selected hypotheses for statistical validation.

## Milestones

1. Static demo with one synthetic sales dataset.
2. Dataset upload and automatic profiling.
3. Signal detectors for missingness, outliers, correlations, and segment
   differences.
4. Hypothesis cards and ranking engine.
5. Insight board and Markdown export.
6. Handoff payload for Sales Data Science Agent.
7. Time-series and before/after signal detection.
8. Portfolio case study with screenshots and example workflow.

## Evaluation

- Does Signal Atlas surface patterns a human analyst would investigate?
- Are weak or low-quality signals ranked lower?
- Does each hypothesis explain why it matters?
- Does each hypothesis include evidence, caveats, and recommended validation?
- Can the handoff payload correctly pre-fill a statistical test?

## Risks

- Generic chart generation may feel too similar to existing BI/notebook tools.
- Ranking can overstate noisy patterns if data quality is not included.
- Users may treat hypotheses as conclusions unless the UI clearly labels them as
  unvalidated.
- Business relevance is hard to infer from column names alone.

## Website Card Copy

Signal Atlas is a hypothesis discovery engine that scans datasets for unusual
patterns, ranks the strongest signals, explains why they may matter, and hands
the best hypotheses to the Sales Data Science Agent for statistical validation.
