# Mona's Project Pulse Dashboard Implementation Plan

## Summary

Build a lightweight, dependency-free static dashboard that helps contributors quickly understand Mona's active projects, ownership, current status, recent activity, priority or risk, and a short contributor-friendly summary.

The implementation will create:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

The app will load project data from JSON, render accessible project cards, and use a polished responsive card layout. The VS Code launch configuration will run `python3 -m http.server 5500` from the `app/` directory and open `index.html`, ensuring the dashboard UI appears instead of a directory listing.

The repository currently contains the agent definitions, exercise brief, workflow checks, dev-container setup, and `.vscode/tasks.json`, but the required application files and `.vscode/launch.json` are intentionally absent. No package manifest, frontend framework, build system, or application test runner is present, so the implementation should use browser-native HTML, CSS, and JavaScript only.

## Roles and Responsibilities

### Orchestrator
- Coordinate the Planner, Designer, and Coder.
- Use this plan as the source of truth for phases, file ownership, dependencies, and validation.
- Dispatch work only when prerequisites are satisfied, prevent overlapping edits, resolve integration issues, and perform the final review.

### Planner
- Research the repository and Project Pulse brief.
- Maintain this plan, define the data contract, UI requirements, ownership boundaries, dependency graph, edge cases, and validation expectations.
- Do not implement application source files.

### Designer
- Own the visual and accessibility direction for `app/styles.css`.
- Define information hierarchy for the dashboard header, project grid, cards, status badges, priority treatment, and contributor summaries.
- Create a polished responsive layout using `.dashboard` and `.project-card`, with readable typography, spacing, contrast, rounded cards, shadows, focus states, and reduced-motion behavior where appropriate.
- Review the integrated page for usability and responsive behavior.

### Coder
- Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- Implement semantic markup and JSON-loading/rendering behavior; reference stylesheet and data; render visible cards with status, activity, priority, owner, and summary.
- Create strict JSON launch configuration named `Run Project Pulse Dashboard`, with `cwd` `${workspaceFolder}/app`, command `python3 -m http.server 5500`, and URL `http://localhost:%s/index.html`.
- Validate the implementation and surface errors explicitly.

## File Assignments

| File | Primary owner | Scope and required outcome |
| --- | --- | --- |
| `docs/project-pulse-plan.md` | Planner, reviewed by Orchestrator | Requirements, phases, assignments, dependencies, ordering, edge cases, validation, and assumptions. |
| `app/index.html` | Coder | Semantic Project Pulse page, stylesheet/data references, JSON loading, visible `project-card` rendering, status/activity/priority output, and loading/error states. |
| `app/styles.css` | Designer | Polished responsive dashboard styling, `.dashboard`, `.project-card`, status and priority treatments, `border-radius`, `box-shadow`, spacing, contrast, and focus states. |
| `app/project-data.json` | Coder, using Designer's display contract | Strict JSON with a top-level `projects` array. Every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short `summary`. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named `Run Project Pulse Dashboard`, `cwd` `${workspaceFolder}/app`, command `python3 -m http.server 5500`, and `serverReadyAction` opening `http://localhost:%s/index.html`. |
| `.vscode/tasks.json` | No change | Preserve the existing Copilot CLI task. |

## Data and UI Contract

`app/project-data.json` uses a top-level `projects` array. Each project contains `name`, `owner`, `status`, `recentActivity`, `priority`, and a short contributor-facing `summary`. Use representative static records because no real dataset is supplied.

`app/index.html` uses the exact page title `Project Pulse`, references `styles.css` and `project-data.json`, provides semantic headings and a dashboard landmark, renders one `.project-card` per project, displays every required field, uses safe text rendering, and provides loading, empty, and visible error states. No additional JavaScript file is required; the small script may remain in `app/index.html`.

## Ordered Implementation Steps

1. **Confirm requirements and shared contract** — Orchestrator and Planner confirm the brief, dependency-free static approach, JSON fields, CSS hooks, semantic structure, and launch behavior. This precedes all implementation.
2. **Implement independent foundations in parallel** — Designer creates `app/styles.css`; Coder creates `app/project-data.json` and `.vscode/launch.json`. Both depend only on the shared contract and have separate file ownership.
3. **Implement HTML integration sequentially** — Coder creates `app/index.html` after the data fields and CSS hooks exist; implement semantic markup, loading/empty/error states, safe rendering, and all visible project fields.
4. **Perform integrated review** — Orchestrator with Designer and Coder checks HTML/CSS hooks, JSON fields, launch path, visual hierarchy, responsive behavior, contrast, focus states, and data error handling. Cross-boundary fixes require explicit assignment.
5. **Validate the finished dashboard** — Orchestrator and Coder run structural, JSON, launch, browser, responsive, and accessibility checks; no git operations are part of implementation.

## Dependencies

### Runtime dependencies

- Python 3 for `python3 -m http.server 5500`.
- Browser or VS Code preview; JavaScript enabled.
- No npm packages, framework, bundler, external API, or network service.

### File dependencies

- `app/index.html` depends on `app/styles.css` and `app/project-data.json`.
- HTML and CSS share `.dashboard` and `.project-card` hooks.
- `.vscode/launch.json` depends on `app/index.html` and must serve from `${workspaceFolder}/app` so relative data resolution works.

## Parallel and Sequential Work Decisions

### Parallel

After Step 1, Designer can implement `app/styles.css` while Coder independently implements `app/project-data.json` and `.vscode/launch.json` because these primary file scopes do not overlap.

### Sequential

Research and contract definition precede implementation. `app/index.html` follows agreement on JSON fields and CSS hooks. Integrated review follows all implementation. Final validation follows review and fixes. Cross-file corrections are coordinated by the Orchestrator; agents must not concurrently edit overlapping files.

## Edge Cases and Error Handling

- Missing/unreadable data or invalid JSON: show a visible, meaningful error instead of a blank page.
- Missing `projects` array: treat as invalid; empty array: show an explicit empty state.
- Missing optional summary: use a sensible fallback; missing required fields: validate explicitly rather than silently misrepresenting data.
- Unknown status/priority: preserve text with neutral treatment.
- Long content: wrap without overflow; narrow viewports: reflow without horizontal scrolling.
- Special characters: use safe text APIs, not raw HTML interpolation.
- Keyboard focus must remain visible; status/priority must not rely on color alone; honor reduced-motion preferences.
- Port 5500 conflicts should surface clearly and be resolved consistently if configuration changes.

## Validation Expectations

### File and syntax

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse both JSON files with `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
- Confirm launch JSON has no comments and existing exercise validation remains compatible.

### Static content

- HTML has exact `Project Pulse` title, references both assets, includes `project-card`, and renders `status`, `recentActivity`, and `priority`.
- CSS includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Data has top-level `projects`; every record has required fields.
- Launch JSON includes its required name, app cwd, Python server command, and `index.html` target.

### Runtime and accessibility

- Start the launch configuration and confirm the server runs from `app/` and opens `index.html`, not a directory listing.
- Confirm heading, multiple cards, and JSON data are visible; exercise loading/error states; test narrow and wide viewports.
- Confirm semantic headings/landmarks, text labels alongside visual status/priority, visible keyboard focus, readable contrast, and safe wrapping.

## Open Questions and Assumptions

- No production dataset was provided, so use representative static data until Mona supplies real records.
- The brief requires a contributor-friendly summary but does not list `summary` among mandatory fields; this plan assumes adding it is acceptable.
- No brand palette or design system was found; Designer chooses a restrained accessible palette.
- No framework or test runner exists; use native HTML, CSS, and browser JavaScript.
- The launch configuration may use the VS Code `node-terminal` launch style to run the Python server, subject to installed debugger support.
- Preserve `.vscode/tasks.json`; filtering, sorting, editing, persistence, authentication, and live GitHub data are out of scope.
