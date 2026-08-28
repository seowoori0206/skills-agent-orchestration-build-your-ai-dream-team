# Agent team

This project uses a custom team of four specialized agents orchestrated through **GitHub Copilot CLI in a Codespace** to build Mona's Project Pulse dashboard.

## Agents

### Orchestrator
- **Model:** Claude Opus 4.7 (copilot)
- **Responsibility:** Coordinates Planner, Coder, and Designer agents. Breaks down complex requests into tasks, manages parallelism, and ensures integrated results.
- **Location:** `.github/agents/orchestrator.agent.md`

### Planner
- **Model:** Claude Opus 4.7 (copilot)
- **Responsibility:** Creates implementation plans by researching the codebase, documentation, dependencies, and edge cases. Produces ordered steps, file assignments, and identifies work that can run in parallel.
- **Location:** `.github/agents/planner.agent.md`

### Coder
- **Model:** GPT-5.5 (copilot)
- **Responsibility:** Implements code-oriented tasks with clear structure, explicit errors, and testable behavior. Writes code, fixes bugs, and creates support configuration.
- **Location:** `.github/agents/coder.agent.md`

### Designer
- **Model:** Gemini 3.1 Pro (copilot)
- **Responsibility:** Handles UI/UX, accessibility, information architecture, and visual design. Creates polished dashboards with responsive layouts and visual clarity.
- **Location:** `.github/agents/designer.agent.md`

## Orchestration Model

The team operates through GitHub Copilot CLI in a Codespace:
1. The **Orchestrator** receives the user's request and delegates to specialists
2. The **Planner** researches and creates a detailed implementation strategy
3. The **Coder** and **Designer** execute their specialized tasks in parallel or sequentially based on file scope and dependencies
4. The **Orchestrator** verifies integration and reports outcomes
5. All git operations remain under learner control through Copilot CLI prompts
