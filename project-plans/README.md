# Project Enhancement Plan

This folder contains future project specs for the portfolio website. Each file
is designed to become a real standalone project later: start with the MVP,
build in public, then link the finished repository from the website.

## Recommended Website Enhancements

1. Add a dedicated `Projects` page or expand the current project cards into
   richer case-study previews.
2. Add a status label for each project: `Concept`, `Building`, `Prototype`,
   `Shipped`, or `Archived`.
3. Link each project card to its planning file until the real repository exists.
4. Add a short "What I learned" section once a project has a working prototype.
5. Add screenshots, architecture diagrams, and demo GIFs as each project matures.
6. Keep the homepage concise and move deeper technical detail into project pages.

## Project Options

- [Enterprise Analytics Automation Harness](enterprise-analytics-automation-harness.md): an anonymized agent-enabled workflow for Power BI maintenance, database data-quality checks, and sprint documentation.
- [AI Job Discovery Assistant](ai-job-discovery-assistant.md): a local-first job discovery and ranking workflow with user-controlled bookmarking and an in-progress Ollama integration.
- [Signal Atlas](signal-atlas.md): a hypothesis discovery engine that ranks dataset signals and hands them to statistical validation.
- [Sales Data Science Agent](data-science-agent.md): an Excel-based analytics agent for customer sales data and statistical tests.
- [Multi-Agent Stock Market Research System](stock-market-agent.md): an 11-agent investment research system with strategy agents, company stability research, and five-year price analysis.
- [Multilingual OCR On Kubernetes](kubernetes-ml-deployment.md): an OCR service for English and Indian languages deployed with Docker and Kubernetes.

## Portfolio Positioning

Together, these projects show a clear arc:

- Software engineering: designing reliable workflows, validation paths, review gates, and maintainable automation.
- Enterprise automation: applying AI-assisted orchestration to recurring analytics and documentation work without exposing confidential context.
- Responsible AI: keeping people in control of agent-assisted job discovery, using local models deliberately, and respecting platform boundaries.
- Data thinking: finding candidate patterns, ranking them, and turning them into testable hypotheses.
- AI systems: designing agents that reason over data and tools.
- Finance curiosity: applying agents to noisy, high-stakes information.
- Engineering depth: deploying and maintaining model-backed services with modern infrastructure.

## Build Order

1. `Signal Atlas`: easiest to demo visually and a natural feeder into the Sales Data Science Agent.
2. `Sales Data Science Agent`: builds naturally on dataset exploration and practical business analytics.
3. `Multilingual OCR On Kubernetes`: proves engineering maturity through a real inference service.
4. `Multi-Agent Stock Market Research System`: save for later because finance workflows require careful disclaimers, evaluations, and data-source decisions.
