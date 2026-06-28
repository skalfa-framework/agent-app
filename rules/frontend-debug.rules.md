---
trigger: always_on
---

# Frontend Debug Rules
Version: 1.0.0

## Purpose

This document defines **MANDATORY frontend debugging rules** for projects using **Skalfa App**. These rules ensure that all UI bugs are reproduced, analyzed, and verified using the integrated browser and multiple interaction simulations.

If a bug fix violates these rules $\rightarrow$ **the fix MUST NOT be considered valid**.

---

## 1. Core Debugging Principles

### 1.1 Reproduce First
*   Do NOT attempt to fix a bug before reproducing it.
*   The primary tool for reproduction is the **integrated browser**.

### 1.2 Minimal Scoped Fixes
*   Fixes MUST be as small as possible.
*   Do NOT perform visual redesigns, refactor unrelated components, or change API contracts while fixing a bug.
*   Fixes must stay strictly within the affected component/page.

### 1.3 Safe Boundaries
*   Do NOT modify backend code or mock API behaviors in production code.
*   Ensure that all changes remain inside the writable paths.

---

## 2. Browser-Based Verification Protocol

For every frontend runtime bug analysis and verification, the agent MUST perform a **List of Simulations** and record them in the activity/bug report.

### 2.1 Mandatory Minimum Simulations
Even if not explicitly requested in the user's prompt, the agent MUST test the following scenarios:
1.  **Page Accessibility (Halaman Bisa Dibuka)**: Verify the page loads successfully without blank screen crashes. Ensure loading skeletons/spinners appear and transition correctly.
2.  **Interactive Functions (Simulasi Fungsi)**: Simulate and test all interactive elements on the page (e.g., clicking buttons, opening/closing modals, switching tabs, filtering tables) through multiple distinct scenarios.
3.  **Form Input Variations (Uji Coba Pengisian Form)**: For any forms, test with multiple different input variations:
    *   *Happy Path*: Valid data submission.
    *   *Validation Failures*: Missing required fields, invalid formats (e.g., email), or boundary value violations (too long/short).
4.  **Infinite Loop Check (Bebas Infinite Loop)**: Verify that the component does NOT trigger infinite re-renders or infinite API call loops (commonly caused by incorrect `useEffect` dependency arrays).
5.  **Hanging Action Check (Tidak Ada Aksi Menggantung)**: Ensure that all asynchronous actions (such as form submissions or API triggers) resolve correctly and do not hang indefinitely (e.g., a submit button stuck in a "loading" state forever due to unhandled promise rejections or missing state resets).

### 2.2 Verify Console & Network
*   Check the browser console for errors, warnings, or unhandled exceptions.
*   Check the network tab to verify that API requests are sent with the correct payloads and that responses are handled correctly.

### 2.3 Verify States
*   Verify that the UI correctly displays:
    *   **Loading states** (skeletons or spinners during fetch).
    *   **Error states** (user-friendly messages when API fails).
    *   **Empty states** (clear indicators when no data is returned).

---

## 3. Verification & Finalization

### 3.1 Static Verification
*   After applying a fix, the agent MUST run:
    ```bash
    bun tsc --noEmit
    ```
*   Any compiler error MUST be fixed before proceeding.

### 3.2 Runtime Verification
*   Refresh the browser and re-run all simulation scenarios.
*   The fix is only valid when **all scenarios** behave correctly without console errors.

### 3.3 Event Recording
*   Every bug lifecycle transition must be recorded in `.agent/records/ledger.jsonl`:
    *   On reproduction: Record `BUG_FOUND`.
    *   On resolution: Record `BUG_RESOLVED`.
*   Update `.agent/records/state.json` to reflect the active/resolved bugs.
