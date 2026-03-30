# Feature: Note Categories with Sidebar Navigation

| Field | Value |
|---|---|
| **code** | `note-categories` |
| **status** | `draft` |
| **priority** | _awaiting confirmation_ |
| **github_tracking** | https://github.com/SSWDemoOrg/flatnotes/issues/3 |
| **prototype_status** | `approved` |
| **engineering_status** | `not_started` |
| **engineering_owner** | — |
| **engineering_started_at** | — |

---

## Problem

Flatnotes stores every note as a flat list with no structural organization beyond tags and search. As users accumulate notes, there is no way to visually group related notes — making it hard to find and manage notes by topic, project, or area of life. Tags help with search-time filtering but don't provide persistent, glanceable organization in the UI.

## Outcome

Users can create named **categories**, assign each note to exactly one category, and browse notes by category through a **collapsible left sidebar**. Uncategorized notes remain accessible under a default "All Notes" / "Uncategorized" group. The existing tag system is untouched and continues to work independently.

## User stories

1. **As a user**, I want to create, rename, and delete categories so I can organize my notes by topic.
2. **As a user**, I want to assign a note to a category (or change/remove its category) so each note lives in one logical group.
3. **As a user**, I want a left sidebar listing my categories with note counts so I can browse notes by group at a glance.
4. **As a user**, I want to collapse/expand the sidebar so it doesn't take up space when I don't need it.
5. **As a user**, I want uncategorized notes to appear under a default group so nothing is "lost."
6. **As a user**, I want my category assignment to persist across sessions and devices so the organization is durable.

## Functional requirements

### FR-1: Category CRUD

- Users can **create** a category by providing a name (1–64 characters, unique, case-insensitive).
- Users can **rename** a category; all notes in that category update automatically.
- Users can **delete** a category; notes in the deleted category become uncategorized (not deleted).
- Category names are trimmed of whitespace; duplicates (case-insensitive) are rejected with a user-friendly error.

### FR-2: Assign note to category

- A note can belong to **exactly one** category or be uncategorized (default).
- Assignment can happen from the note editor (a dropdown/picker) and optionally via drag-and-drop in the sidebar.
- Changing or removing a category does not modify note content or tags.

### FR-3: Sidebar UI

- A **left sidebar** displays all categories in alphabetical order, each showing its note count.
- Selecting a category filters the main content area to show only notes in that category.
- An **"All Notes"** entry at the top shows every note regardless of category.
- An **"Uncategorized"** entry shows notes with no assigned category.
- The sidebar is **collapsible** (toggle button or hamburger icon); collapsed state persists in localStorage.
- The sidebar is responsive: hidden by default on small screens, shown as an overlay or drawer on tap.

### FR-4: Persistence

- Categories and note-to-category mappings are stored **server-side** so they sync across devices.
- Storage mechanism must work with the existing flat-file system (no external database required). Recommended approach: a `categories.json` file in the notes directory, or a frontmatter field in each note's `.md` file.

## Non-functional requirements

| ID | Requirement |
|---|---|
| **NFR-1** | Sidebar renders in < 200 ms with up to 100 categories and 1,000 notes. |
| **NFR-2** | Category operations (create, rename, delete, assign) complete in < 500 ms. |
| **NFR-3** | No breaking changes to existing API contracts — new endpoints only. |
| **NFR-4** | Accessible: sidebar navigable by keyboard (Tab/Arrow keys); ARIA roles for tree/list navigation. |
| **NFR-5** | Dark mode support consistent with existing theme system. |
| **NFR-6** | Works with all auth modes (none, read_only, password, totp). In read-only mode, category management is hidden but browsing is available. |

## API surface (proposed)

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/categories` | List all categories with note counts |
| `POST` | `/api/categories` | Create a category `{name}` |
| `PATCH` | `/api/categories/{name}` | Rename a category `{newName}` |
| `DELETE` | `/api/categories/{name}` | Delete a category (notes become uncategorized) |
| `PATCH` | `/api/notes/{title}` | Extended: accept optional `{category}` field |

## Dependency register

| ID | Dependency | Status | Impact |
|---|---|---|---|
| **DEP-01** | Flatnotes flat-file storage — must support a metadata file or frontmatter extension | Open | Storage design choice needed during technical inception |
| **DEP-02** | Whoosh search index — category field may need indexing for filtered search | Open | Index schema update during implementation |
| **DEP-03** | PrimeVue component library — sidebar/tree components available | Resolved | PrimeVue v3.53.1 ships Sidebar, Tree, and Menu components |

## Out of scope

- Nested / hierarchical categories (sub-categories) — potential future enhancement.
- Drag-and-drop reordering of categories (alphabetical only for v1).
- Bulk-assign notes to categories.
- Category-level permissions or sharing.
- Any changes to the existing tag system.

## Open questions

_None at this time — ready for prototype review and priority confirmation._
