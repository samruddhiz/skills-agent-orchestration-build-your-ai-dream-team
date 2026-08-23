# Project Pulse Implementation Plan

## Goal

Build a lightweight, dependency-free Project Pulse dashboard that helps contributors quickly see active projects, owners, current status, recent activity, priority or risk, and a short contributor-friendly summary. The app must present the dashboard from `app/index.html` and launch through the VS Code configuration named `Run Project Pulse Dashboard`, rather than showing a server directory listing.

## Implementation phases and file assignments

### 1. Define the shared interface

- **Orchestrator:** Confirm the project data shape, visible information hierarchy, CSS hooks, and ownership boundaries before implementation.
- **Designer and Coder:** Agree on the stable `.dashboard` and `.project-card` integration hooks, status/priority treatment, and the required project fields.

### 2. Design the dashboard

- **Designer:** Own `app/styles.css`.
- Define responsive layout, readable typography and spacing, project-card presentation, status badges, priority/risk emphasis, contrast, keyboard focus states, rounded corners, and shadows.
- Keep `.dashboard` and `.project-card` selectors explicit and reusable.

### 3. Implement the static dashboard

- **Coder:** Own `app/index.html`.
  - Use the exact `Project Pulse` title and reference `styles.css` and `project-data.json`.
  - Provide semantic, accessible markup and a visible dashboard container.
  - Render project cards using `project-card`, including `name`, `owner`, `status`, `recentActivity`, `priority`, and the contributor-friendly summary.
  - Surface loading and data errors clearly instead of leaving a blank page.
- **Coder:** Own `app/project-data.json`.
  - Use a top-level `projects` array.
  - Give every project `name`, `owner`, `status`, `recentActivity`, and `priority`.
  - Include multiple representative projects with realistic values.
- **Coder:** Own `.vscode/launch.json`.
  - Use strict JSON with no comments.
  - Define `Run Project Pulse Dashboard`.
  - Serve `${workspaceFolder}/app` with `python3 -m http.server 5500`.
  - Configure `serverReadyAction` to open `http://localhost:%s/index.html`.

### 4. Integrate and review

- **Orchestrator:** Coordinate the Designer and Coder outputs, resolve contract mismatches, and keep each agent within its assigned files.
- **Designer:** Review visual hierarchy, responsive behavior, accessibility, and styling hooks.
- **Coder:** Review HTML/data compatibility, launch behavior, explicit error handling, and runnable-app support.

## Responsibilities

- **Designer:** Owns the visual direction, information hierarchy, responsive layout, accessibility presentation, and `app/styles.css`; does not modify Coder-owned files unless explicitly reassigned.
- **Coder:** Owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; implements data rendering, semantic markup, strict JSON, launch behavior, and implementation-level validation.
- **Orchestrator:** Owns delegation, sequencing, integration decisions, final validation, and handoff; does not blur file ownership between agents.

## Dependencies

- The shared interface contract must be agreed before implementation begins.
- The HTML rendering logic depends on the schema in `app/project-data.json`.
- `app/index.html` and `app/styles.css` depend on shared `.dashboard`, `.project-card`, status, and priority hooks.
- `.vscode/launch.json` depends on the app being served from `app/` and must target `index.html`; it is otherwise independent of styling.
- Integration review and final validation depend on both Designer and Coder work being complete.

## Parallel work decisions

After the shared interface contract is confirmed, these non-overlapping tasks may run in parallel:

- Designer creates `app/styles.css`.
- Coder creates `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.

Data-schema agreement must precede HTML rendering, and integration review, preview testing, and final validation must run sequentially after both work streams finish.

## Edge cases and risks

- Preview through HTTP because loading JSON from a `file://` URL may fail.
- Handle loading, empty, malformed, missing, or partially populated project data with an informative state.
- Ensure long project names, activity text, and priority labels wrap without breaking the layout.
- Preserve readable contrast, visible keyboard focus, and usable narrow-screen behavior.
- Avoid duplicate or conflicting CSS ownership.
- Ensure the launch configuration opens `/index.html`, not the app directory listing, and account for port `5500` conflicts.

## Validation expectations

From the repository root:

1. Run `bash scripts/validate-exercise.sh`.
2. Parse both JSON files with `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
3. Use focused checks to confirm:
   - `app/index.html` contains `Project Pulse`, references both app assets, includes `.dashboard` and `project-card`, and renders `name`, `owner`, `status`, `recentActivity`, and `priority`.
   - `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
   - `app/project-data.json` contains the top-level `projects` array and required project fields.
   - `.vscode/launch.json` contains `Run Project Pulse Dashboard`, the app working directory, the HTTP server command, `serverReadyAction`, and the `/index.html` target.
4. Start the launch configuration or `python3 -m http.server 5500 --directory app` and confirm the browser displays the Project Pulse dashboard rather than a directory listing.
