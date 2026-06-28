---
description: Frontend Development Workflow
---

# Frontend Development Workflow
Version: 1.3.0

This workflow defines a strict, schema-governed **frontend planning and development** process for a frontend project with **Skalfa App**.

The agent MUST obey workspace boundaries, component layering rules, formatting rules, and artifact rules. No UI/UX improvisation is allowed.

---

## WORKSPACE DIRECTORY MAPPING

*   **Frontend Project Root**: `./skalfa-app/` (or current project folder containing `app/`)
*   **Target Writable Paths**: 
    *   `app/**` (Next.js App Router pages, modules, and their nested `_constructs`, `_structures`, and `_services` folders)
    *   `components/construct.components/**` (Shared composed UI components, e.g., `*-*.construct.tsx`)
    *   `components/structure.components/**` (Shared page layout structures, e.g., `*-*.structure.tsx`)
    *   `contexts/**`
    *   `styles/**`
    *   `schema/**`
    *   `README.md` (for project outline updates)
*   **Read-Only Paths**: 
    *   `utils/**`
    *   `components/base.components/**`
*   **Agent Folder**: `./.agent/` (configured as `agent-frontend/` during development)
*   **Records Directory**: `./.agent/records/`

---

## EVENT LEDGER & STATE PROTOCOL

Every major stage transition MUST be recorded in `./.agent/records/ledger.jsonl` as an append-only JSON line. After writing to the ledger, `./.agent/records/state.json` must be compiled/updated to reflect the current active tasks, bugs, and UI specs.

All detailed logs, plans, and code diffs must be stored as separate files in `./.agent/records/activities/` and referenced via relative paths in the ledger payload.

---

## STAGE 1 — FRONTEND DEVELOPMENT PLANNING

### Step 1.1 — Context & Knowledge Alignment (README-First)
*   The agent MUST read the project's root `README.md` to understand the overall project outline, UI modules, and design guidelines.
*   Verify how the requested UI feature fits into the existing modules and outline described in `README.md`.
*   **Knowledge Mapping**: The agent MUST read the Knowledge Registry in `./.agent/knowledges/registry.md`. Identify and read ONLY the specific knowledge files (e.g., `form-supervision.md`, `api.md`) that are directly relevant to the components and utilities required for the task. Reading unrelated knowledge files is forbidden to prevent context bloat.


### Step 1.2 — Outline Adjustment (If Applicable)
*   If the requested UI feature introduces changes, adjustments, or deviations from the existing project outline, the agent MUST update the corresponding sections in `README.md` (or draft the proposed updates) to keep the project outline accurate.

### Step 1.3 — Plan Creation & Detailing
*   Based on the project outline in `README.md`, define:
    *   Affected pages (routes under `app/`)
    *   Affected components (constructs, structures)
    *   Required UI states (loading, error, empty, success)
    *   Required user interactions
    *   API endpoints consumed and context providers used
*   Create a plan detail file: `./.agent/records/activities/act-<num>-plan-<feature>.md`.
*   Record the event in `./.agent/records/ledger.jsonl`:
    ```json
    {"timestamp": "TIMESTAMP", "agent": "AGENT_NAME", "event": "FEATURE_PLANNED", "payload": {"feature": "FEATURE_NAME", "plan_file": "./.agent/records/activities/act-<num>-plan-<feature>.md"}}
    ```

---

## STAGE 2 — IMPLEMENTATION

### Step 2.1 — Component Placement & Naming
*   **Structures (Page Layouts/Containers)**:
    *   Shared structures: Place in `components/structure.components/` as `<slug>.structure.tsx`.
    *   Page-specific structures: Place in `app/(module)/_structures/` as `<slug>.structure.tsx`.
*   **Constructs (Small Composed Elements)**:
    *   Shared constructs: Place in `components/construct.components/` as `<slug>.construct.tsx`.
    *   Page-specific constructs: Place in `app/(module)/_constructs/` as `<slug>.construct.tsx`.
*   **Services (Business/API Logic)**:
    *   Page-specific services: Place in `app/(module)/_services/` as `<slug>.service.ts`.
*   **Naming Conventions**:
    *   All component files and service files MUST use slug format (e.g., `status-badge.construct.tsx`, `booking.service.ts`).
    *   Component function names should be descriptive and match the file's intent (no `Component` suffix is required).

### Step 2.2 — Coding & Styling
*   **Separation of Logic & Presentation (Hook Pattern)**: Extract all business logic, state management (`useState`, `useReducer`), side effects (`useEffect`), and API calls into a custom React hook in `_services/` (e.g., `useBooking` in `booking.service.ts`). The UI component/page must only contain presentational JSX and call the hook to retrieve data.
    *   *Exception*: Simple usage of built-in utility hooks (like `useApi()`) does NOT require extraction and can be called directly in the component (e.g., `const { data, loading } = useApi(...)`). If there are multiple `useApi` calls, rename the destructured variables (e.g., `const { data: dataProduct } = useApi(...)`).
*   **Magic Components First**: Always evaluate and use magic components like `FormSupervisionComponent` and `TableSupervisionComponent` before building custom UIs.
*   **Imports**: Import from `@components`, `@utils`, and `@contexts`.
*   **Icons**: Use FontAwesome icons from `@fortawesome/free-solid-svg-icons`.
*   **Strict Coding Style**:
    *   Align object keys vertically in block form (column-based alignment using spaces).
    *   Do not break lines for imports, function calls, or chaining unless the line exceeds reasonable length.

---

## STAGE 3 — STATIC DEBUGGING (MANDATORY)

Before running the app, the agent MUST verify that it passes TypeScript compile check:
*   Run the TypeScript check in the frontend project root:
    ```bash
    bun tsc --noEmit
    ```
*   If errors occur, apply static bug fixing rules immediately. Do not run the app until 0 errors are reported.

---

## STAGE 4 — RUNTIME DEBUGGING & SIMULATION (MANDATORY)

*   Start the frontend development server.
*   Use the integrated browser to navigate to the modified feature path.
*   Execute **multiple simulation scenarios** (e.g., login success, login failure with incorrect password, network timeouts).
*   Verify loading states, error states, and empty states.
*   If runtime errors or UI discrepancies occur, apply fixes and re-verify.

---

## STAGE 5 — CODE REVIEW & FINALIZATION

### Step 5.1 — Code Review
*   Verify that:
    *   No files outside the writable paths were modified (especially `base.components` or `utils`).
    *   All file names follow the slug convention.
    *   Vertical alignment is consistently applied.
*   Create a review report and diff patch:
    *   Report: `./.agent/records/activities/act-<num>-review-<feature>.md`
    *   Patch: `./.agent/records/activities/act-<num>-diff-<feature>.patch`

### Step 5.2 — Ledger Finalization
*   Update the feature map in `./.agent/records/features.md` with any new/modified pages and routes.
*   Record the completion event in `ledger.jsonl`:
    ```json
    {"timestamp": "TIMESTAMP", "agent": "AGENT_NAME", "event": "FEATURE_COMPLETED", "payload": {"feature": "FEATURE_NAME", "review_file": "./.agent/records/activities/act-<num>-review-<feature>.md", "patch_file": "./.agent/records/activities/act-<num>-diff-<feature>.patch"}}
    ```
*   Update `./.agent/records/state.json` to mark the feature/task as `DONE`.

