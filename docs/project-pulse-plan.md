# Project Pulse Implementation Plan

The custom Planner invocation could not run because its configured model was unsupported. This fallback plan was prepared from `.github/project-pulse-brief.md`, `.github/agents/*`, and the repository's Step 2/Step 3 requirements.

## Goal

Create a lightweight static Project Pulse dashboard for contributors that shows active project information, owners, status, recent activity, priority/risk, and a short summary in a polished, accessible, responsive card layout. The app will use plain HTML, CSS, and JSON with no build step. The Orchestrator coordinates the workflow using GitHub Copilot CLI in a Codespace.

## Implementation phases

1. **Fix the shared selector and data contract**
   - The Orchestrator, Designer, and Coder should work together to define the stable DOM hooks and CSS selectors used by the dashboard: `.dashboard`, `.project-card`, status, summary, owner, activity, and priority elements.
   - Confirm that `app/index.html` owns the document structure and card rendering and that `app/styles.css` owns their visual styling.
   - Confirm the JSON shape is a top-level `projects` array and agree the field values used for representative sample data, including how the short summary is supplied or derived.

2. **Implement in parallel after the contract is fixed**
   - **Designer — `app/styles.css`:** own all styling and the `.dashboard` and `.project-card` selectors. Create clear typographic hierarchy, spacing, card borders, border-radius, shadowing, status and priority affordances, responsive columns, and narrow-screen behavior. Include visible keyboard focus styles and readable text/background contrast. Avoid relying on color alone for status or priority.
   - **Coder — `app/index.html`, `app/project-data.json`, `.vscode/launch.json`:**
     - `app/index.html`: create the document with the exact HTML title `Project Pulse`, reference `styles.css`, reference/load `project-data.json`, and render visible cards from `projects`. Use the `project-card` class for every project card and expose status, recentActivity, priority, owner, name, and a short summary.
     - `app/project-data.json`: provide representative project records containing `name`, `owner`, `status`, `recentActivity`, and `priority`; include a summary field if used by the agreed markup and available in every record.
     - `.vscode/launch.json`: create strict JSON with no comments. Add the configuration named `Run Project Pulse Dashboard`, serve from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and use `serverReadyAction` to open `http://localhost:%s/index.html`, preventing a directory listing.

3. **Integrate and validate sequentially**
   - After both assignments are complete, Coder and Designer work should meet at the CSS/DOM contract, then the Orchestrator should verify the real HTML, CSS, JSON, and launch files together.
   - Confirm cards render only after JSON loads successfully, empty and error states remain understandable, and the launch URL reaches `index.html`.

## File ownership

- **Designer:** owns `app/styles.css` and is responsible for accessibility, visual hierarchy, and responsive, polished design.
- **Coder:** owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; must implement markup, data rendering, sample data, and runnable preview configuration.
- The Orchestrator coordinates scope and integration but does not replace the specialists' file ownership.

## Dependencies

The CSS/DOM selector contract in Phase 1 is mandatory before parallel implementation. It depends on reading the brief and Step 2/Step 3 requirements and must identify the card container, `.dashboard`, `.project-card`, and field element or class conventions.

Once the contract is fixed:

- `app/index.html` and `app/styles.css` can be developed in parallel because they have separate file ownership.
- `app/project-data.json` can be developed in parallel with CSS and HTML, provided it uses the agreed top-level and field names.
- `.vscode/launch.json` depends on the app directory and entry filename but can be prepared in parallel after the app layout is agreed.
- Integration is sequential and occurs only after both agents finish their assigned files.
- Launch and manual preview validation follow integration and are not concurrent with file edits.

## Parallel work decisions

- **Can run in parallel:** after the selector contract is agreed, Designer can implement `app/styles.css` while Coder implements `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- **Must remain sequential:** the contract first; Coder and Designer integration second; launch configuration and manual preview validation only after the files exist. Do not let parallel work change ownership or redefine selectors without coordinating the contract.

## Edge cases

- Handle unavailable, missing, malformed, or invalid JSON with an informative visible message rather than silently showing an empty dashboard.
- Render a clear empty-state message when `projects` is an empty array.
- Validate required project fields and avoid treating missing values as successful cards; provide reasonable safe fallback text and preserve card structure.
- Normalize or clearly label unknown status and priority values rather than assuming that a badge's preset color applies.
- Verify that long names, summaries, owner names, and activity text wrap cleanly.
- Preserve usable card layout at narrow widths, without horizontal overflow or clipped text.
- Use semantic headings and landmarks, readable status and priority labels, keyboard-visible focus styles, and descriptive screen-reader text where icons or visual-only elements are used.
- Maintain readable contrast for normal text, badges, focus indicators, and interactive elements.

## Validation expectations

- Confirm `app/index.html` has the exact title `Project Pulse`, links `styles.css`, references or loads `project-data.json`, and renders cards from the top-level `projects` data using `project-card`.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, plus polished border-radius, box-shadow, spacing, status/priority treatment, and responsive behavior.
- Parse `app/project-data.json` as JSON and verify the top-level `projects` key and required `name`, `owner`, `status`, `recentActivity`, and `priority` fields for every representative project.
- Parse `.vscode/launch.json` as strict JSON and verify the exact `Run Project Pulse Dashboard` name, app-directory `cwd`, `python3 -m http.server 5500` command, and `http://localhost:%s/index.html` serverReadyAction.
- Run the launch configuration, verify the browser opens the dashboard UI at `/index.html` rather than a directory listing, stop the server, and exercise narrow and normal viewport widths.
- Perform focused static and JSON checks plus manual preview; no existing app implementation, build, or test suite was present, so repository-wide automated tests are not available for this exercise.
- Existing Step 2 and Step 3 workflow expectations should be met: plan content, delegated file ownership, static app requirements, launch behavior, and a runnable preview.

## Assumptions and open questions

- Representative sample project data is sufficient unless requirements later specify real project records.
- A static HTML/CSS/JSON implementation is reasonable; no framework, library, package install, or build tooling is assumed unless later requirements demand one.
- The short summary may use an explicit JSON `summary` field or an agreed UI summary derived from existing fields; because the brief explicitly requires a summary but the required JSON field list does not include `summary`, this should be resolved during the Phase 1 contract.
- Status and priority vocabularies are open; representative values and fallbacks should be clear and accessible, with unknown values handled rather than hidden.
