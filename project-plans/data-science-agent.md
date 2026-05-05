# Sales Data Science Agent

## Concept

Sales Data Science Agent is a guided analytics system that accepts an Excel file
with customer IDs and sales data, lets users choose from a menu of statistical
tests, runs the selected analysis, stores the results in a database, and returns
a clear business-friendly summary.

The agent should feel like a junior data scientist with guardrails: it profiles
the data, checks whether the chosen test is appropriate, asks for missing inputs,
executes the analysis, stores reproducible results, and explains what the result
does and does not mean.

## Portfolio Pitch

This project demonstrates practical data science automation: Excel ingestion,
statistical testing, agentic tool orchestration, result persistence, explainable
summaries, and production-style analytics workflows.

## Target Users

- Business analysts working with customer and sales spreadsheets.
- Small teams that want guided statistical testing without writing code.
- Students learning when to use different tests and how to interpret results.

## Input Data

Initial input format: `.xlsx` Excel workbook.

Expected columns:

- `customer_id`: unique customer identifier.
- `date`: transaction date, order date, invoice date, or reporting period.
- `sales`: revenue, sales amount, order value, or similar numeric metric.

Optional useful columns:

- `group`: treatment/control, region, segment, campaign, channel, or cohort.
- `units`: quantity sold.
- `product_id`: product identifier.
- `region`: customer or sales region.
- `campaign_id`: marketing campaign identifier.
- `is_treatment`: boolean treatment indicator.
- `intervention_date`: date when a campaign, policy, or product change happened.

## User Flow

1. User uploads an Excel file.
2. Agent profiles the workbook and maps columns.
3. Agent shows data quality warnings: missing values, duplicates, bad dates,
   nonnumeric sales values, extreme outliers, and suspicious customer IDs.
4. User chooses a test from a menu.
5. Agent asks only for missing required inputs, such as group column,
   intervention date, test direction, or confidence level.
6. Agent validates assumptions for the selected test.
7. Agent runs the analysis.
8. Results, parameters, warnings, and dataset metadata are stored in a database.
9. Agent summarizes the result for the user and links to a detailed report.

## Test Menu

### MVP Tests

- T test: compare average sales between two groups.
- Paired T test: compare before/after sales for the same customers.
- Chi-square test: test relationships between categorical variables.
- Correlation test: measure association between sales and another numeric field.
- Linear regression: estimate relationship between sales and predictors.
- ANOVA: compare average sales across more than two groups.

### Advanced Tests

- BSTS: Bayesian structural time series for causal impact style before/after
  analysis with a clear intervention date.
- Mann-Whitney U test: nonparametric alternative to two-sample T test.
- Kruskal-Wallis test: nonparametric alternative to ANOVA.
- Difference-in-differences: estimate treatment impact using treatment and
  control groups before and after an intervention.
- Time-series decomposition: identify trend, seasonality, and residual patterns.
- Customer segmentation: cluster customers by sales behavior.

## Skills

Each analysis should be implemented as a skill with a consistent interface.

### `excel_ingestion_skill`

- Reads `.xlsx` files.
- Detects sheets.
- Normalizes column names.
- Produces a clean dataframe and ingestion report.

### `data_profile_skill`

- Summarizes row count, columns, data types, missing values, duplicates, and
  basic sales distribution.
- Detects whether the dataset is customer-level, transaction-level, or
  aggregated time series.

### `column_mapping_skill`

- Suggests mappings for customer ID, date, sales, group, treatment, and segment.
- Lets the user confirm or override mappings.

### `test_recommendation_skill`

- Recommends valid tests based on available columns and user goal.
- Explains why unavailable tests are disabled.

### `assumption_check_skill`

- Checks requirements for the selected test.
- Examples: group count, sample size, missingness, normality, equal variance,
  independent observations, paired observations, time-series frequency, and
  intervention date.

### `statistical_test_skill`

- Runs the selected statistical test using approved libraries.
- Returns structured results, plots, parameters, warnings, and interpretation
  helpers.

### `result_storage_skill`

- Stores uploads, dataset metadata, selected test, parameters, result objects,
  warnings, generated charts, and summary text.

### `summary_skill`

- Produces a plain-English summary.
- Includes business interpretation, statistical caveats, and suggested next
  steps.
- Uses only the request-scoped evidence bundle for the current analysis job.
- Cites result fields, diagnostics, and warnings from that job instead of
  relying on memory or prior analyses.
- Refuses to summarize claims that are not present in the current result object.

## Hooks

Hooks are lifecycle checkpoints around every analysis.

- `before_ingest`: validate file size, extension, and workbook readability.
- `after_ingest`: store upload metadata and sheet summary.
- `before_test`: verify required columns and user-provided parameters.
- `after_test`: store raw result, diagnostics, warnings, and runtime.
- `before_summary`: assemble result context, assumptions, and caveats.
- `after_summary`: store final summary and mark job as complete.
- `on_error`: store failure reason, traceback category, and user-safe message.

## Grounded Summarization Rules

The summary hook must be designed to prevent hallucination and cross-request
contamination. It should summarize only the findings produced for the current
request.

### Request-Scoped Evidence Bundle

Before summarization, create a single immutable evidence bundle for the current
analysis job.

The bundle should include:

- `job_id`
- `dataset_id`
- `upload_id`
- selected `test_type`
- user-confirmed parameters
- column mapping
- row counts used and rows excluded
- statistical result fields
- confidence intervals
- p-values
- effect sizes
- diagnostics and assumption checks
- warnings and caveats
- chart data summaries
- runtime metadata

The summarizer receives this bundle and nothing else unless explicitly needed
for style instructions.

### Summary Contract

The summary should return structured output:

- `headline`: one-sentence result.
- `key_findings`: bullets grounded in result fields.
- `business_interpretation`: cautious interpretation of what the result may
  suggest.
- `statistical_details`: test statistic, p-value, confidence interval, effect
  size, and sample size when available.
- `caveats`: assumption warnings and data quality limitations.
- `next_steps`: suggestions based only on current findings.
- `evidence_refs`: references to exact fields in the evidence bundle.

### Anti-Hallucination Guardrails

- Do not use results from previous jobs.
- Do not use uploaded files from other requests.
- Do not mention columns that are not present in the current column mapping.
- Do not infer causality unless the selected test supports causal language and
  the assumptions passed.
- Do not invent business context, campaign names, customer segments, dates, or
  metrics.
- Use "the analysis found" only when the result object contains the finding.
- Use "the data does not show enough evidence" when results are inconclusive.

### Validation Hook

Add a `summary_validation_hook` after summary generation.

It should check:

- Every numeric claim in the summary appears in the evidence bundle.
- Every column name in the summary exists in the current dataset mapping.
- The summary's `job_id`, `dataset_id`, and `upload_id` match the current
  request.
- Causal language is blocked for non-causal tests like basic T tests,
  correlation, and simple regression.
- Prior result IDs are not referenced unless the user explicitly requested a
  cross-analysis comparison.
- The caveats section includes all critical assumption warnings.

If validation fails, the summary is regenerated with the validation errors as
constraints. If it fails twice, return a conservative template summary built
directly from the result fields without free-form generation.

### Database Isolation

- Store summaries with the exact `job_id` they describe.
- Retrieve summaries by `job_id`, not by latest upload or user session alone.
- Use foreign keys from `analysis_results.job_id` to `analysis_jobs.id`.
- Add a unique result row per job to avoid overwriting summaries from other
  requests.
- Log the evidence bundle hash used for summary generation.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript.
- Backend: FastAPI.
- Data processing: Pandas or Polars.
- Excel support: openpyxl.
- Statistical tests: scipy, statsmodels, scikit-learn.
- BSTS options: start with a simplified causal impact approach using
  statsmodels/PyMC later; treat full BSTS as an advanced milestone.
- Database: Postgres for production-like learning, SQLite for earliest local
  prototype.
- File storage: local filesystem for MVP, object storage later.
- Job execution: synchronous for MVP, Celery/RQ/Arq later for longer tests.

## Database Design

### `uploads`

- `id`
- `filename`
- `uploaded_at`
- `file_hash`
- `row_count`
- `sheet_names`
- `status`

### `datasets`

- `id`
- `upload_id`
- `selected_sheet`
- `column_mapping_json`
- `profile_json`
- `quality_warnings_json`

### `analysis_jobs`

- `id`
- `dataset_id`
- `test_type`
- `parameters_json`
- `status`
- `created_at`
- `completed_at`
- `runtime_ms`

### `analysis_results`

- `id`
- `job_id`
- `result_json`
- `diagnostics_json`
- `charts_json`
- `summary_text`
- `caveats_json`

## Architecture

1. Excel upload enters the backend.
2. Ingestion skill reads workbook and stores upload metadata.
3. Profiling and column mapping skills prepare the dataset.
4. Frontend displays available tests based on data shape.
5. User selects a test and confirms required parameters.
6. Assumption checker validates the request.
7. Statistical test skill runs the analysis.
8. Result storage skill writes outputs to the database.
9. Evidence bundle builder creates request-scoped summary context.
10. Summary skill creates the user-facing explanation.
11. Summary validation hook checks grounding and request isolation.
12. Frontend displays summary, detailed result table, charts, and caveats.

## MVP Scope

- Upload one Excel sheet.
- Detect customer ID, date, sales, and group columns.
- Run two-sample T test and correlation test.
- Store upload metadata, job metadata, and results in SQLite or Postgres.
- Generate a concise grounded result summary tied to the current `job_id`.
- Display prior analysis history.

## Milestones

1. Excel upload and sheet preview.
2. Data profiling and column mapping.
3. Test menu with disabled/enabled states.
4. T test implementation.
5. Correlation and linear regression implementation.
6. Database persistence for jobs and results.
7. Grounded summary generation with evidence-bundle validation.
8. ANOVA, chi-square, and paired T test.
9. BSTS or causal impact style workflow.
10. Job queue for long-running analyses.
11. Portfolio case study with screenshots and sample workbook.

## Evaluation

- Does the agent choose valid tests based on the uploaded data?
- Does it block tests when assumptions or required columns are missing?
- Are p-values, confidence intervals, and effect sizes reported correctly?
- Are summaries understandable to a nontechnical user?
- Is every summary claim grounded in the current job's result object?
- Does the system prevent summaries from referencing other users, uploads, or
  prior requests?
- Can a user reproduce an old analysis from stored parameters and results?
- Does the system avoid overstating causality?

## Risks

- Users may choose tests that do not match their data.
- Sales data may have repeated customers, seasonality, outliers, and missing
  periods that violate simple test assumptions.
- BSTS requires careful setup, especially intervention dates, control series,
  and time aggregation.
- Generated summaries may overclaim unless grounded in stored diagnostics.
- Summaries may accidentally mix context from prior requests unless the
  summarizer receives only a request-scoped evidence bundle.
- Excel files can have messy headers, merged cells, hidden sheets, and mixed
  data types.

## Open Product Questions

- Should the agent recommend tests automatically, or should users always choose?
- Should the first version support only one sheet or multiple sheets?
- Should results be stored per uploaded file, per user account, or per session?
- Should the app include authentication from the start?
- Should BSTS be implemented early, or should simpler tests come first?

## Website Card Copy

Sales Data Science Agent turns customer sales Excel files into guided
statistical analysis. Users upload data, choose tests like T tests, regression,
ANOVA, or BSTS, and receive stored, reproducible results with clear summaries
and caveats.
