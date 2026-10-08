# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s Project Pulse dashboard as a lightweight static app for contributors to quickly see each project’s name, owner, current status, recent activity, priority or risk, and a short summary. The app should open as a polished, accessible, responsive dashboard—not as a server directory listing.

The repository currently has no app implementation or frontend framework, package manifest, or existing app conventions to extend. The brief specifies three app files and a VS Code launch configuration, so keep the implementation dependency-free using HTML, CSS, browser-native JavaScript, and JSON.

## Ordered implementation steps

### 1. Confirm the shared data and markup contract

Agree on the fields and selectors before parallel implementation begins:

- `app/project-data.json` has a top-level `projects` array.
- Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Include a short contributor-friendly summary for each project, as requested by the dashboard brief. The brief does not prescribe a field name for the summary; settle that name before implementing the renderer.
- Each rendered project uses the `project-card` class. The page includes a `.dashboard` container and status/priority treatments.
- The page loads the JSON over HTTP and uses it to render visible project cards. Do not rely on opening `index.html` with `file://`.

**File scope:** No file changes; this is a contract and design handoff.

**Dependency:** Required before the Designer and Coder start work in parallel.

### 2. Design the dashboard and implement its stylesheet

The **Designer** defines the visual hierarchy and responsive behavior, then owns `app/styles.css`. Style a clear page title, readable project cards, distinct status and priority/risk badges, and useful spacing. Include accessible contrast, visible keyboard focus, and suitable small-screen behavior. Honor the agreed `.dashboard` and `.project-card` hooks.

**File assignment:** `app/styles.css` — Designer.

**Dependency:** Depends on Step 1’s selector and content contract. The stylesheet can be developed in parallel with the data and launch configuration in Step 3.

### 3. Implement the project data and launch configuration

The **Coder** owns both files in this step:

- `app/project-data.json` — provide multiple representative projects with non-empty values for all required fields and the agreed summary field. Keep the file strict, parseable JSON.
- `.vscode/launch.json` — provide a strict JSON launch configuration named **Run Project Pulse Dashboard**. Serve from `${workspaceFolder}/app` using `python3 -m http.server 5500`, and configure the ready action to open `http://localhost:%s/index.html`. Use a valid VS Code launch configuration and a ready-output pattern that matches the selected launch mechanism.

**File assignments:** `app/project-data.json` and `.vscode/launch.json` — Coder.

**Dependencies:** Depends on Step 1’s agreed data contract. These files can be implemented in parallel with Step 2 because they have separate file ownership.

### 4. Implement the page and data-driven rendering

The **Coder** owns `app/index.html`. Build a semantic page with the exact title **Project Pulse**, a visible page heading, a reference to `styles.css`, and browser-native JavaScript that loads `project-data.json` and renders a `project-card` for each project. Display every required field, including status, `recentActivity`, and priority, along with the agreed summary. Keep the HTML hooks consistent with the Designer’s stylesheet.

Provide clear visible feedback when data cannot be loaded or when the project list is empty; do not leave the main content blank or show raw JSON.

**File assignment:** `app/index.html` — Coder.

**Dependencies:** Must follow Step 1’s contract and coordinate with Step 2 so the markup uses the Designer’s selectors. It can start while Steps 2 and 3 are underway only if the agreed contract is stable; final integration must wait for their outputs.

### 5. Integrate and validate the dashboard

The Orchestrator checks that the four assigned files fit together: the page loads the data, the rendered cards match the stylesheet hooks, and the launch configuration serves and opens the app. The Coder addresses implementation issues within the assigned files; the Designer addresses visual or responsive issues in `app/styles.css`.

**File scope:** Fixes remain within `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`, owned as assigned above.

**Dependency:** Requires Steps 2–4 to be complete.

## Responsibilities

- **Designer:** Own `app/styles.css`; define and implement information hierarchy, visual design, accessible status and priority treatments, readable spacing, and responsive behavior. Report design decisions and any markup hooks the Coder must honor.
- **Coder:** Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; implement data-driven project cards, handle data-loading and empty states, and create a launch configuration that serves from `app/` and opens `index.html`. Validate the assigned implementation and report any remaining risks.
- **Orchestrator:** Coordinate the shared contract, keep file ownership explicit, manage dependencies, integrate the work, and verify the final dashboard against this plan.
- **Planner:** Establish the phases, dependencies, ownership, risks, and validation expectations before implementation.

## Dependencies and work ordering

### Can run in parallel

After Step 1 establishes the shared contract:

- Designer can implement `app/styles.css`.
- Coder can implement `app/project-data.json` and `.vscode/launch.json`.

These tasks have separate file ownership and no direct implementation dependency on each other.

### Must run sequentially or be coordinated

- Step 1’s shared contract must precede parallel implementation, especially the summary field name and markup hooks.
- `app/index.html` must use the agreed data shape and the Designer’s selectors. Its implementation can begin once the contract is stable, but final integration depends on the stylesheet and JSON being available.
- Integrated validation must follow completion of all four files.
- Browser verification of the launch configuration must follow configuration implementation.

## Risks and edge cases

- **Unspecified summary field:** The brief requires a short summary but lists only five mandatory keys. Agree on a summary key before producing the data and renderer; do not silently omit the summary.
- **Static file loading:** Fetching JSON from a page opened through `file://` may fail in the browser. Use the provided local HTTP server for preview and testing.
- **Data-load failures:** The JSON may be missing, malformed, or unavailable. Show a clear in-page error instead of an empty dashboard or an unhandled exception.
- **Empty or incomplete data:** Handle an empty `projects` array and avoid rendering broken cards if a field is absent or blank. Keep sample data complete and meaningful.
- **Status and priority values:** The brief does not define allowed values. Choose consistent values for sample data; ensure labels remain understandable and visually distinguishable without depending on color alone.
- **Launch readiness:** A server-ready pattern that does not match the Python server’s actual output can prevent the browser from opening. Verify the launch configuration with the actual VS Code launch mechanism.
- **Port conflicts or unavailable tools:** Port `5500` may already be in use, and Python 3 or a suitable browser/debug configuration may be unavailable. Surface these as environment issues and verify an alternate port only if the exercise requirements permit it.
- **Accessibility and small screens:** Check contrast, focus visibility, readable text, semantic structure, and card layout at narrow viewport widths.
- **No established frontend patterns:** The repository has no existing app structure or frontend dependencies. Avoid adding frameworks or build tooling unless the scope changes.

## Validation expectations

1. Confirm all required files exist:
   - `app/index.html`
   - `app/styles.css`
   - `app/project-data.json`
   - `.vscode/launch.json`
2. Parse both JSON files with `python3 -m json.tool`.
3. Inspect `app/index.html` to confirm it:
   - Uses the exact visible title **Project Pulse**.
   - References `styles.css` and `project-data.json`.
   - Renders visible project cards with the `project-card` class.
   - Displays each project’s name, owner, status, `recentActivity`, priority, and agreed summary.
   - Provides understandable error and empty states.
4. Inspect `app/styles.css` to confirm it includes `.dashboard` and `.project-card`, polished card styling such as `border-radius` and `box-shadow`, and responsive and accessible states.
5. Confirm the data file has a top-level `projects` array and each project contains the required fields and agreed summary.
6. Confirm `.vscode/launch.json` is strict JSON, includes **Run Project Pulse Dashboard**, serves from `app/`, and opens `http://localhost:%s/index.html` rather than the server root.
7. Run **Run Project Pulse Dashboard** in VS Code. Verify the browser shows the dashboard, cards load from JSON, the page does not show a directory listing, and the layout remains usable at a narrow viewport.
8. Test the empty-data and failed-data-load states where practical. Stop the preview server after browser verification.

## Open questions

- What field name should represent the contributor-friendly summary (for example, `summary`)?
- Which sample project statuses and priority/risk labels should be used? The brief does not define an allowed vocabulary.
- Which VS Code launch mechanism is available in the target Codespace, and does its server-ready pattern match Python’s output as configured?
