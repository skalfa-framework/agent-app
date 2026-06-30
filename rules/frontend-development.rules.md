---
trigger: always_on
---

# Frontend Development Rules
Version: 1.3.0

## Purpose

This document defines **MANDATORY frontend engineering rules** for projects using **Skalfa App**. These rules are derived directly from Skalfa App's component-driven architecture and establish the standard patterns for directory structure, component layering, magic components, styling, and code formatting.

If an implementation violates these rules $\rightarrow$ **the task MUST NOT be considered DONE**.

---

## 1. Mandatory Research & Planning Phase (Registry-First)

Before creating any plan (such as `implementation_plan.md`) or performing any codebase analysis/research:
*   The agent **MUST** read the local Knowledge Registry at `./.agents/knowledges/registry.md` to understand the available technical services and utilities.
*   Identify which specific utilities (e.g. API, Auth, Form, Caching, Socket) or technical concepts are required for the task.
*   The agent **MUST** open and read those specific knowledge files in `./.agents/knowledges/` **BEFORE** exploring the codebase or proposing any changes. This prevents unnecessary codebase searches and ensures alignment with Skalfa App patterns.
*   **Clarification & No-Assumptions**: If the user prompt or the project's `README.md` is ambiguous, incomplete, or lacks specific details (such as UI fields, specific designs, or business logic edge cases), the agent **MUST NOT** make silent assumptions. The agent **MUST** ask the user for clarification directly in the chat or list the questions under the `## Open Questions` section of the implementation plan and wait for feedback.

---

## 2. Core Principles (Binding)

### 1.1 Component-Driven Architecture
*   UI must be built by composing components, not by duplicating JSX.
*   Reusable components are preferred over page-specific implementations.

### 1.2 Naming Conventions (Slug-like)
*   All component files in `constructs` and `structures` (both shared and page-specific) MUST use slug-like names.
*   All component files MUST end with `.construct.tsx` or `.structure.tsx`.
*   All service files MUST end with `.service.ts`.
*   Component function names should be descriptive and match the file's intent (no specific suffix like `Component` is required).
    *   *Correct File*: `status-badge.construct.tsx`
    *   *Correct Function*: `export function StatusBadge() { ... }`

### 1.3 Layering Definitions
*   **Structures** (`_structures/` or `components/structure.components/`): ONLY used to separate pages that have multiple page structures or layout containers.
*   **Constructs** (`_constructs/` or `components/construct.components/`): ONLY used for small, composed UI elements (e.g., status badges, cards, buttons).
*   **Services** (`_services/`): Page-specific business or API logic.
*   **Base Components** (`components/base.components/`): Primitive UI elements (e.g., Accordion, Breadcrumb, Button, Input, Modal, Table). **Read-Only**.

### 1.4 Component & Service Placement
*   **Shared/Global**:
    *   Shared constructs go to `components/construct.components/` as `<slug>.construct.tsx`.
    *   Shared structures go to `components/structure.components/` as `<slug>.structure.tsx`.
*   **Page-Specific/Local**:
    *   Local constructs go to `app/(module)/_constructs/` as `<slug>.construct.tsx`.
    *   Local structures go to `app/(module)/_structures/` as `<slug>.structure.tsx`.
    *   Local services go to `app/(module)/_services/` as `<slug>.service.ts`.

### 1.6 Separation of Logic and Presentation (Hook Pattern)
*   All UI presentation (HTML/JSX) MUST be separated from business and interaction logic.
*   Logic (state management, API fetching, event handlers, side effects) MUST be encapsulated in a custom React Hook located in a service file (e.g., `export function useBooking() { ... }` in `booking.service.ts` under `_services/`).
*   The page or component file (`page.tsx`, `<slug>.construct.tsx`, `<slug>.structure.tsx`) MUST only contain presentational JSX. It should call the custom hook to retrieve the needed states and handlers.
*   Direct declarations of `useState`, `useReducer`, `useEffect`, or API calls inside the UI component itself are forbidden.
*   **Exception (Built-in Utility Hooks)**: Simple usage of built-in utility hooks (like `useApi()`) does NOT require extraction into a custom hook. They can be called directly in the presentational component (e.g., `const { data, loading } = useApi(...)`). If there are multiple `useApi` calls in the same file, rename the destructured variables to avoid conflicts (e.g., `const { data: dataProduct } = useApi(...)`).

---

## 3. Coding Patterns

### 2.1 Reuse of Existing Components (Component Reuse Discipline)
*   As long as an existing component (either in `base.components` or `construct.components`) can satisfy the requirement, the agent MUST reuse it.
*   It is strictly forbidden to create new custom components or write raw HTML/JSX for UI elements whose functionality is already represented by existing components.
*   Do not casually create new components unless the requirement is truly custom and cannot be achieved by extending or composing existing components.

### 2.3 Reuse of Existing Utilities (Utility Reuse Discipline)
*   Skalfa App provides a rich set of built-in utilities (e.g., `api`, `auth`, `form`, `idb`, `cn`, `conversion`, `encryption`, `resource`, `shortcut`, `socket`, `table`, `validation`) via the `@utils` path alias.
*   The agent MUST reuse these existing utilities. Writing custom helper functions or reinventing logic (like custom date conversion, manual API fetchers, or raw IndexedDB wrappers) is strictly forbidden.
*   Do not casually create custom utility files.

---

## 4. Code Formatting (Vertical Alignment)

*   Object keys, variable assignments, and type definitions MUST be aligned vertically in block form (column-based alignment). Use spaces to align `:` and `=` consistently within the same block.
    *   *Example*:
        ```typescript
        fields={[
          {
            col          :  12,
            construction :  {
              name        :  "username",
              label       :  "Username",
              placeholder :  "Masukkan username",
            }
          }
        ]}
        ```
*   **Single-Line Imports**: All import statements (including multiple destructured modules/bindings) MUST be written on a single line. Breaking imports into multiple lines (e.g., placing bindings on new lines with enters) is strictly forbidden.
    *   *Correct*: `import { one, two, three } from '...'`
    *   *Incorrect*:
        ```typescript
        import {
          one,
          two,
          three
        } from '...'
        ```
*   Do NOT break lines for function calls or chaining unless the line becomes hard to read or exceeds reasonable length.

---

## 5. Code Modification Boundaries

### 5.1 Writable Paths (agent MAY modify)
*   `app/**` (Next.js App Router pages, modules, and local `_constructs`, `_structures`, `_services` folders)
*   `components/construct.components/**`
*   `components/structure.components/**`
*   `contexts/**`
*   `styles/**`
*   `schema/**`
*   `.agent/records/**`

### 5.2 Read-Only Paths (agent MUST NOT modify)
*   `utils/**`
*   `components/base.components/**`

If a fix requires changes inside read-only paths, the agent MUST NOT modify the file, but must report it to the human as `fix_but_need_human`.

### 5.3 README.md Modification Constraints
*   The agent is allowed to add new features to the checklist in `README.md` (e.g., adding `- [ ] New Feature` when planning).
*   **The agent MUST NOT mark any checklist items as completed (changing `[ ]` to `[x]`) in `README.md`.** Checking or marking the status of features in the `README.md` checklist is reserved exclusively for humans.

---

## 6. Definition of DONE

A frontend task is considered **DONE** only when:
1.  Skalfa App patterns and slug-like naming are strictly followed.
2.  Component layering is respected (`structures` vs `constructs`).
3.  Magic components (`FormSupervisionComponent`) are used where applicable.
4.  Code formatting is vertically aligned.
5.  The TypeScript compiler check (`bun tsc --noEmit`) passes with 0 errors.
6.  The feature is verified in the integrated browser under multiple simulated scenarios.
7.  The feature map in `.agent/records/features.md` is updated with any new/modified pages and routes.
8.  The ledger (`.agent/records/ledger.jsonl`) and state (`.agent/records/state.json`) are updated.

