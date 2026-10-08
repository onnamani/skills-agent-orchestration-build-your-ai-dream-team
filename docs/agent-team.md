# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team: gets a plan, divides work into scoped phases, delegates to specialists, manages dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the codebase, docs, dependencies, and edge cases, then produces a practical implementation plan with file assignments, sequencing, risks, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard's usability, accessibility, information hierarchy, interactions, responsive behavior, and visual design, including clear project cards and status/priority treatment. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements assigned application logic and support configuration with clear, testable behavior; for Project Pulse, can set up the app launch configuration when assigned. | `.github/agents/coder.agent.md` |
