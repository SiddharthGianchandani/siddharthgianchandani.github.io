# Data Science Agent

## Concept

Data Science Agent is an AI assistant that helps users move from an unfamiliar
dataset to a reproducible analysis plan. It should profile data, ask clarifying
questions, suggest analytical paths, write code, and produce notebooks or reports.

## Portfolio Pitch

This project demonstrates agent design, tool use, data science workflow
automation, evaluation, and human-in-the-loop product thinking.

## Target Users

- Students learning data science.
- Analysts starting a new project.
- Builders who want quick dataset triage before deeper modeling.

## MVP Scope

- Upload a CSV.
- Agent profiles schema, missingness, data types, and summary statistics.
- Agent asks 2-3 clarifying questions about the user's goal.
- Agent proposes an analysis plan.
- Agent generates Python code for exploratory analysis.
- User can run generated code in a controlled notebook-like environment.

## Stretch Features

- Multi-agent workflow: profiler, analyst, visualization critic, report writer.
- Automatic model baseline recommendations.
- Data quality warnings and leakage checks.
- Notebook export.
- Experiment tracking integration.
- Support for SQL databases.

## Suggested Tech Stack

- Frontend: React, Vite, TypeScript.
- Backend: FastAPI or Next.js API routes.
- Data tools: Pandas, Polars, scikit-learn, DuckDB.
- Agent framework: start simple with direct tool calls; consider LangGraph later.
- Execution sandbox: Jupyter kernel, Pyodide, Modal sandbox, or a restricted server process.

## Agent Tools

- `profile_dataset`: returns schema, missingness, cardinality, sample rows.
- `make_chart`: generates a visualization spec.
- `run_python`: executes approved analysis code.
- `write_notebook`: exports final analysis steps.
- `critique_analysis`: checks for leakage, weak assumptions, or unclear claims.

## Architecture

1. User uploads data and states a goal.
2. Backend stores the dataset temporarily.
3. Profiler tool summarizes the dataset.
4. Agent creates an analysis plan and requests confirmation.
5. Code-generation tool produces analysis cells.
6. Execution tool runs cells and returns results.
7. Report writer summarizes findings and caveats.

## Milestones

1. Dataset upload and profiling.
2. Non-agent analysis-plan generator.
3. Agent loop with tool calls.
4. Safe Python execution.
5. Notebook/report export.
6. Evaluation suite with 5-10 datasets and expected analysis behaviors.

## Evaluation

- Does the agent ask useful clarifying questions?
- Does generated code run without edits?
- Are conclusions grounded in actual outputs?
- Does the agent avoid overclaiming from limited data?

## Risks

- Generated code can be unsafe if execution is not sandboxed.
- The agent may hallucinate columns or patterns.
- Analysis quality needs evaluation, not vibes.

## Website Card Copy

Data Science Agent is an AI analysis partner that profiles datasets, asks smart
questions, generates reproducible notebooks, and helps turn raw data into
defensible insight.
