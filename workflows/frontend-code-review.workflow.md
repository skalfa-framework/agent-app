---
description: Frontend Code Review Workflow
---

# Frontend Code Review Workflow
Version: 1.3.0

This workflow defines a strict, non-destructive **code review and structural alignment process** for frontend projects using **Skalfa App**.

The workflow ensures that the codebase conforms to Skalfa App folder structures, slug-like naming conventions, and vertical formatting rules.

---

## WORKSPACE DIRECTORY MAPPING

*   **Frontend Project Root**: `./skalfa-app/` (or current project folder containing `app/`)
*   **Agent Folder**: `./.agent/`
*   **Records Directory**: `./.agent/records/`

---

## REVIEW CHECKLIST

The reviewer agent MUST analyze all modified and new files against the following criteria:

### 1. Folder Structure & Component Layering
*   Pages MUST be placed only in the `app/` directory following Next.js App Router conventions (e.g., `app/login/page.tsx`).
*   Components and services MUST be placed in:
    *   **Shared Paths**:
        *   `components/construct.components/` for shared composed UI elements (e.g., status badges, cards).
        *   `components/structure.components/` ONLY for separating pages that have multiple page structures or layout containers.
    *   **Page-Specific Paths**:
        *   `app/(module)/_constructs/` for page-specific composed UI elements.
        *   `app/(module)/_structures/` ONLY for separating page-specific layouts or containers.
        *   `app/(module)/_services/` for page-specific business/API logic.
*   **Separation of Logic & Presentation (Hook Pattern)**: No business logic, state (`useState`, `useReducer`), side effects (`useEffect`), or API calls are allowed directly inside UI presentational components or page files. All logic MUST be encapsulated in a custom React hook inside `_services/` (e.g., `useBooking` in `booking.service.ts`).
    *   *Exception*: Simple usage of built-in utility hooks (like `useApi()`) does NOT require extraction. If multiple `useApi` are used, verify that destructured variables are renamed (e.g., `const { data: dataProduct } = useApi(...)`).

### 2. File Naming (Slug-like)
*   **File Names**:
    *   All files in shared/local `constructs` MUST use slug-like naming with the `.construct.tsx` suffix (e.g., `status-badge.construct.tsx`).
    *   All files in shared/local `structures` MUST use slug-like naming with the `.structure.tsx` suffix (e.g., `booking-header.structure.tsx`).
    *   All service/helper files MUST use slug-like naming with the `.service.ts` suffix (e.g., `booking-notification.service.ts`).

### 3. Pattern Compliance
*   **Magic Components**: Verify that common CRUD interfaces or forms utilize `FormSupervisionComponent` or `TableSupervisionComponent` rather than custom JSX duplication.
*   **Icons**: Ensure FontAwesome icons from `@fortawesome/free-solid-svg-icons` are used instead of custom SVG elements.
*   **Path Aliases**: Use `@components`, `@utils`, and `@contexts`. Do not use relative imports like `../../components`.

### 4. Code Formatting (Vertical Alignment)
*   Object keys, variable assignments, and type definitions MUST be aligned vertically in block form (column-based alignment using spaces).
    *   *Example of correct alignment*:
        ```typescript
        fields={[
          {
            col: 12,
            construction: {
              name: "username",
              label: "Username",
              placeholder: "Masukkan username",
            }
          }
        ]}
        ```
*   **Single-Line Imports**: Verify all import statements are written on a single line. Do not allow multi-line imports (no wrapping of curly braces).
*   Do not break lines for function calls or chaining unless the line exceeds reasonable length or becomes hard to read.

---

## STAGE 1 — EXECUTION

### Step 1.1 — Run Code Review
*   Scan all modified files in the current commit or task scope.
*   Verify each file against the checklist above.
*   Identify any violations (e.g., camelCase filenames, unaligned colons).

### Step 1.2 — Apply Alignment / Refactoring
*   If violations are found:
    *   Rename files to match the slug-like convention (`.construct.tsx`, `.structure.tsx`, `.service.ts`).
    *   Adjust spacing to enforce vertical alignment.
*   Verify that these changes do NOT alter the functional behavior of the UI.

### Step 1.3 — Finalize Review Report
*   Create a code review report: `./.agent/records/activities/act-<num>-review-report.md`.
*   Record the `CODE_REVIEW_COMPLETED` event in `./.agent/records/ledger.jsonl`:
    ```json
    {"timestamp": "TIMESTAMP", "agent": "AGENT_NAME", "event": "CODE_REVIEW_COMPLETED", "payload": {"report_file": "./.agent/records/activities/act-<num>-review-report.md"}}
    ```
