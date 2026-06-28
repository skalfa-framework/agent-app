---
name: frontend_analyze_runtime_bug
description: Analyze frontend runtime failures using browser-based observation.
---

# Frontend Analyze Runtime Bug Skill

## Goal
Identify the cause of frontend runtime errors by observing browser behavior, network requests, and console logs.

## Allowed Tools & Actions
*   Open the integrated browser.
*   Navigate to specified feature paths.
*   Read console errors and warnings.
*   Inspect network requests and responses.
*   Capture screenshots.

## Step-by-Step Instructions

1.  **Read Feature Map**: Read `.agent/records/features.md` to identify the URL path of the failing feature and obtain the test credentials (username/password) for logging in.
2.  **Open Browser**: Launch the integrated browser, navigate to the identified feature path, and log in using the obtained credentials.
3.  **Simulate Interactions**:
    *   Perform multiple interaction flows and compile a **List of Tried Simulations** which must at least include:
        *   *Page Accessibility*: Verify page opens successfully without crashing and loading states work correctly.
        *   *Interactive Functions*: Test UI actions (buttons, modals, tabs) through multiple scenarios.
        *   *Form Input Variations*: Try multiple different input fillings (valid data, empty fields, invalid validations).
        *   *Infinite Loop Check*: Verify that there are no infinite re-renders or infinite API call loops.
        *   *Hanging Action Check*: Verify async actions resolve without hanging (e.g., submit button stuck in loading).
3.  **Observe**:
    *   Console errors and stack traces.
    *   Network request failures (HTTP status codes, response payloads).
    *   Visual UI issues (broken layouts, missing loading/error states).
4.  **Correlate**: Match the browser observations and console stack traces to the specific page, component, or utility in the frontend source code.
5.  **Report & Record**:
    *   Create a bug analysis report: `.agent/records/activities/act-xxx-bug-analysis-xxx.md`.
    *   Append the `BUG_FOUND` event to `.agent/records/ledger.jsonl`.
    *   Update `.agent/records/state.json` to list the bug as active.

## Forbidden Actions
*   Modifying backend code or API behaviors.
*   Editing production frontend logic during the analysis phase.
*   Performing UI redesigns or altering the approved layout.
