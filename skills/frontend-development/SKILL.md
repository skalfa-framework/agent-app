---
name: frontend_development
description: Implement frontend features according to an approved frontend plan in Skalfa App.
---

# Frontend Development Skill

## Goal
Implement frontend code strictly according to the approved implementation plan, Skalfa App component patterns, and vertical alignment formatting rules.

## Allowed Tools & Actions
*   Edit frontend source files in writable paths (`app/**`, `components/construct.components/**`, `components/structure.components/**`, `contexts/**`, etc.).
*   Open the integrated browser to verify UI behavior.
*   Run TypeScript compiler checks:
    ```bash
    bun tsc --noEmit
    ```

## Step-by-Step Instructions

1.  **Read the Plan**: Open the latest planning file in `.agent/records/activities/act-xxx-plan-xxx.md`.
2.  **Verify Component Layering**:
    *   **Shared/Global Components**:
        *   Place small composed components in `components/construct.components/` as `<slug>.construct.tsx`.
        *   Place page layout structures in `components/structure.components/` as `<slug>.structure.tsx`.
    *   **Page-Specific/Local Components & Services**:
        *   Place local constructs in `app/(module)/_constructs/` as `<slug>.construct.tsx`.
        *   Place local structures in `app/(module)/_structures/` as `<slug>.structure.tsx`.
        *   Place local services in `app/(module)/_services/` as `<slug>.service.ts`.
3.  **Implement UI & Logic**:
    *   Extract all state, effects, and API calls into a custom React hook inside `_services/` (e.g., `useBooking` in `booking.service.ts`).
    *   *Exception*: Simple usage of built-in utility hooks (like `useApi()`) does NOT require extraction and can be called directly in the component. If there are multiple `useApi` calls, rename the destructured variables (e.g., `const { data: dataProduct } = useApi(...)`).
    *   Keep presentational components (`.construct.tsx`, `.structure.tsx`, `page.tsx`) strictly focused on rendering JSX/HTML using the data from the custom hook or utility hook.
    *   Use magic components like `FormSupervisionComponent` and `TableSupervisionComponent` where applicable.
    *   Use FontAwesome icons.
4.  **Format Code**: Apply vertical alignment to object keys (`: `) and variable assignments (`= `) in the block. Keep imports and function calls on a single line unless excessively long.
5.  **Verify Statics**: Run `bun tsc --noEmit` in the project root. Resolve any compiler errors immediately.
6.  **Verify Runtimes**: Open the integrated browser, navigate to the feature path, and verify the UI under multiple simulation scenarios (loading, success, validation error, network error).
7.  **Record Activities**: Save the code diff patch and explanation in `.agent/records/activities/`, and append the `FEATURE_COMPLETED` event to `.agent/records/ledger.jsonl`.

## Forbidden Actions
*   Modifying files in read-only paths (such as `utils/**` or `components/base.components/**`).
*   Improvising UI/UX layouts outside the approved plan.
*   Skipping static verification or browser-based testing.
