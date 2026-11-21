# Agent team

I am using GitHub Copilot CLI in a Codespace to orchestrate the custom agent team for Mona's Project Pulse dashboard. The team keeps planning, design, implementation, and coordination as separate responsibilities.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates Planner, Coder, and Designer; turns the approved plan into dependency-aware phases, assigns file scopes, delegates specialist work, and verifies the integrated dashboard. | [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md) |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and requirements, identifies edge cases and dependencies, assigns file ownership, and produces a practical implementation plan for the static dashboard. | [.github/agents/planner.agent.md](../.github/agents/planner.agent.md) |
| Designer | Gemini 3.7 Flash (copilot) | Designs the dashboard UI/UX, information hierarchy, accessibility, interaction flow, responsive behavior, project cards, status badges, and polished visual layout. | [.github/agents/designer.agent.md](../.github/agents/designer.agent.md) |
| Coder | GPT-5.5 (copilot) | Implements `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`, with clear code and validation of the runnable dashboard. | [.github/agents/coder.agent.md](../.github/agents/coder.agent.md) |
