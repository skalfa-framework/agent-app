---
description: Frontend Runtime Debug Workflow
---

# Frontend Runtime Debug Workflow
Version: 1.0.0

This workflow defines a strict, feature-driven, and ledger-governed **runtime debugging process for a frontend project using Skalfa App**.

The agent MUST use the integrated browser and user interaction simulations to reproduce and verify fixes.

---

## WORKSPACE DIRECTORY MAPPING

*   **Frontend Project Root**: `./skalfa-app/` (or current project folder containing `app/`)
*   **Agent Folder**: `./.agent/`
*   **Records Directory**: `./.agent/records/`

---

## STAGE 1 — REPRODUCTION & ANALYSIS

### Step 1.1 — Start Environment
*   Ensure the frontend development server is running (e.g., `bun run dev` in the frontend project root).

### Step 1.2 — Simulate & Observe
*   The agent MUST read `.agent/records/features.md` to identify the correct URL path of the feature and retrieve the test credentials (username/password) required to log in.
*   Open the integrated browser, navigate to the identified path, log in using the retrieved credentials, and perform **multiple interaction simulations** to compile a **List of Tried Simulations** including:
    *   *Page Accessibility*: Page can be opened without crashing, loading skeletons/spinners display correctly.
    *   *Interactive Functions*: Simulate all UI actions (button clicks, tab switches, modal triggers) through several scenarios.
    *   *Form Input Variations*: Try multiple different input test fillings (valid, empty/invalid validation).
    *   *Infinite Loop Check*: Verify that there are no infinite re-renders or infinite API call loops.
    *   *Hanging Action Check*: Verify that async operations resolve properly without hanging (e.g., submit button stuck in loading state).
*   Observe and capture:
    *   Console errors and warnings.
    *   Network request payloads and response statuses (API consumption).
    *   Visual UI issues (broken layouts, missing loading/error states).

### Step 1.3 — Record Bug
*   Create a bug analysis report: `./.agent/records/activities/act-<num>-bug-analysis-<slug>.md`.
*   Record the `BUG_FOUND` event in `./.agent/records/ledger.jsonl`:
    ```json
    {"timestamp": "TIMESTAMP", "agent": "AGENT_NAME", "event": "BUG_FOUND", "payload": {"bug_id": "BUG-<num>", "feature": "FEATURE_NAME", "analysis_file": "./.agent/records/activities/act-<num>-bug-analysis-<slug>.md"}}
    ```
*   Update `./.agent/records/state.json` to add the bug to the active bugs list.

---

## STAGE 2 — FIXING & VERIFICATION

### Step 2.1 — Apply Minimal Fix
*   Modify the frontend source code in writable paths (`app/**`, `components/construct.components/**`, etc.).
*   Do NOT modify read-only paths (like `utils/**` or `components/base.components/**`). If a change there is required, mark as `need_human`.
*   Maintain vertical alignment and slug-like naming conventions.

### Step 2.2 — Static Verification
*   Run the TypeScript compiler check to ensure no type errors are introduced:
    ```bash
    bun tsc --noEmit
    ```

### Step 2.3 — Runtime Verification
*   Refresh the integrated browser and re-run the **List of Tried Simulations** (page accessibility, interactive functions, form input variations, infinite loop check, hanging action check).
*   Verify that:
    *   All console errors are gone.
    *   Network requests succeed.
    *   The UI behaves correctly under all simulated scenarios with no infinite loops or hanging actions.

---

## STAGE 3 — FINALIZATION

### Step 3.1 — Record Resolution
*   If the bug is resolved:
    *   Create a resolution report and patch file in `activities/`.
    *   Record the `BUG_RESOLVED` event in `./.agent/records/ledger.jsonl`:
        ```json
        {"timestamp": "TIMESTAMP", "agent": "AGENT_NAME", "event": "BUG_RESOLVED", "payload": {"bug_id": "BUG-<num>", "patch_file": "./.agent/records/activities/act-<num>-fix-<slug>.patch", "resolution_file": "./.agent/records/activities/act-<num>-resolution-<slug>.md"}}
        ```
    *   Update `./.agent/records/state.json` to mark the bug status as `RESOLVED`.
