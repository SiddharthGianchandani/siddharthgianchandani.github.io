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

- [Signal Atlas](signal-atlas.md): a data storytelling and signal discovery dashboard.
- [Data Science Agent](data-science-agent.md): an AI assistant that profiles datasets, proposes analysis, and generates reproducible notebooks.
- [Stock Market Agent](stock-market-agent.md): an agentic research system for market monitoring, portfolio watchlists, and risk-aware summaries.
- [Multilingual OCR On Kubernetes](kubernetes-ml-deployment.md): an OCR service for English and Indian languages deployed with Docker and Kubernetes.

## Portfolio Positioning

Together, these projects show a clear arc:

- Data thinking: finding patterns and explaining them.
- AI systems: designing agents that reason over data and tools.
- Finance curiosity: applying agents to noisy, high-stakes information.
- Engineering depth: deploying and maintaining model-backed services with modern infrastructure.

## Build Order

1. `Signal Atlas`: easiest to demo visually and link on the site.
2. `Data Science Agent`: builds naturally on dataset exploration.
3. `Multilingual OCR On Kubernetes`: proves engineering maturity through a real inference service.
4. `Stock Market Agent`: save for later because finance workflows require careful disclaimers, evaluations, and data-source decisions.
