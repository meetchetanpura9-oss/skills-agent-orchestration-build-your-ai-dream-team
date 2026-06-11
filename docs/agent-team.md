# Agent team

This project uses a small custom agent team to build Mona's Project Pulse dashboard. The team members, their models, responsibilities, and agent definition files are listed below.

- Orchestrator
  - Model: Claude Opus 4.7 (copilot)
  - Responsibility: Coordinate the Planner, Coder, and Designer; break the work into phases, assign file scopes, run tasks in parallel or sequentially as appropriate, and verify the integrated result.
  - Agent file: .github/agents/orchestrator.agent.md

- Planner
  - Model: Claude Opus 4.7 (copilot)
  - Responsibility: Research the codebase and dependencies, produce a practical implementation plan with ordered steps, file assignments, dependencies, parallelization guidance, edge cases, and validation checks.
  - Agent file: .github/agents/planner.agent.md

- Coder
  - Model: GPT-5.5 (copilot)
  - Responsibility: Implement code and runnable app support within the Orchestrator-assigned file scope; create support configuration when required; validate changes and report risks.
  - Agent file: .github/agents/coder.agent.md

- Designer
  - Model: Gemini 3.1 Pro (copilot)
  - Responsibility: Provide UI/UX, accessibility, information architecture, interaction flow, and visual design for the dashboard; produce deterministic CSS hooks and design tradeoffs.
  - Agent file: .github/agents/designer.agent.md

How the team will work together to build Project Pulse

1. Planner researches the repository and returns a prioritized implementation plan with file-level ownership and dependencies.
2. Orchestrator converts the plan into phases, assigns files to Coder and Designer, and decides which tasks can run in parallel.
3. Designer creates UI/UX assets and CSS suggestions for the assigned files and reports validation recommendations.
4. Coder implements the frontend/backend pieces, adds any required launch/config files for a runnable preview, and runs validation steps.
5. Orchestrator integrates outputs, resolves conflicts, and runs the final verification steps. Any blockers or open questions are reported back for clarification.

Notes

- Work is coordinated via GitHub Copilot CLI in a Codespace; agents do not stage, commit, or push changes—git operations remain under the learner's control.
- Agent definitions live in .github/agents/ as referenced above.
