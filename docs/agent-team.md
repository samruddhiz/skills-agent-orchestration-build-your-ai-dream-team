# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a custom agent team for building Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the project, delegates work to specialists, manages file scopes and dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, then creates implementation plans covering steps, assignments, dependencies, risks, edge cases, and validation. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements dashboard code and logic, fixes bugs, creates assigned runnable-app support files, and validates deterministic, testable behavior. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements UI/UX, accessibility, information hierarchy, responsive behavior, visual clarity, and polished Project Pulse dashboard styling. | `.github/agents/designer.agent.md` |
