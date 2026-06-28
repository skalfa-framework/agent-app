---
name: frontend_code_review
description: Review code structure, naming conventions, and vertical alignment for Skalfa App.
---

# Frontend Code Review Skill

## Goal
Detect and resolve files, folders, and code patterns that violate Skalfa App conventions, ensuring the codebase remains clean, consistent, and vertically aligned.

## Allowed Tools & Actions
*   Read all frontend files.
*   Move or rename files and folders to match conventions.
*   Refactor code formatting to align colons (`:`) and equals signs (`=`).

## Review Checklist

1.  **Folder Structure & Layering**:
    *   Pages must be under `app/`.
    *   **Shared/Global**:
        *   Shared constructs must be in `components/construct.components/`.
        *   Shared structures must be in `components/structure.components/`.
    *   **Page-Specific/Local**:
        *   Local constructs must be in `app/(module)/_constructs/`.
        *   Local structures must be in `app/(module)/_structures/`.
        *   Local services must be in `app/(module)/_services/`.
2.  **File Naming (Slug-like)**:
    *   Verify all files use slug-like names (e.g., `status-badge.construct.tsx`, `booking-header.structure.tsx`, `booking-notification.service.ts`).
    *   No camelCase or PascalCase file names are allowed in the modified scope.
3.  **Pattern Compliance**:
    *   Verify that all state management (`useState`), side effects (`useEffect`), and API calls are extracted into a custom React hook inside `_services/` (with the exception of simple built-in utility hooks like `useApi()`). If multiple `useApi` calls are present in a single file, verify that their destructured variables are renamed (e.g., `const { data: dataProduct } = useApi(...)`).
    *   Verify magic components (`FormSupervisionComponent`, `TableSupervisionComponent`) are used where applicable.
    *   Verify FontAwesome icons are used.
    *   Verify path aliases (`@components`, `@utils`, `@contexts`) are used.
4.  **Code Formatting (Vertical Alignment & Imports)**:
    *   Ensure colons (`:`) in object declarations and equals signs (`=`) in variable assignments are vertically aligned.
    *   Verify all import statements are written on a single line (no multi-line wrapping).
    *   Check that function calls or chaining do not have unnecessary line breaks.

## Step-by-Step Instructions

1.  **Scan Changes**: Review all modified and new files in the current git diff or task scope.
2.  **Identify Violations**: Check each file against the review checklist.
3.  **Apply Refactoring**:
    *   Rename files if they violate the slug-like convention.
    *   Adjust spacing to enforce vertical alignment.
4.  **Report**:
    *   Create a review report: `.agent/records/activities/act-xxx-review-report.md`.
    *   Record the `CODE_REVIEW_COMPLETED` event in `.agent/records/ledger.jsonl`.

## Forbidden Actions
*   Changing functional feature behavior during refactoring.
*   Modifying files in read-only paths (`utils/**`, `components/base.components/**`).
