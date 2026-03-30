# Coding Standards

## Overview
Flatnotes follows Python community conventions (PEP 8 via Black/flake8) for the backend and Prettier for the JavaScript/Vue.js frontend. Standards are practical and minimal — enforced by existing tooling.

## Code Formatting

**Python**: Black + isort
- Line length: 79
- isort profile: black
- Enforced via dev dependencies in Pipfile

**JavaScript**: Prettier
- Prettier with tailwindcss plugin for consistent class ordering
- Config in `prettier.config.js`

## Linting

**Python**: flake8
- Standard flake8 rules
- No custom configuration beyond defaults

**JavaScript**: No linter currently configured

## Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Python variables | snake_case | `user_name`, `is_active` |
| Python functions | snake_case | `get_note_by_title` |
| Python classes | PascalCase | `NoteModel`, `SearchIndex` |
| Python constants | UPPER_SNAKE | `MAX_RESULTS`, `DATA_DIR` |
| Python modules | snake_case | `note_service.py` |
| JS variables | camelCase | `noteTitle`, `isEditing` |
| JS functions | camelCase | `fetchNotes`, `updateSearch` |
| Vue components | PascalCase | `NoteEditor`, `SearchBar` |
| JS files (components) | PascalCase | `NoteEditor.vue` |
| JS files (utilities) | camelCase | `api.js`, `helpers.js` |

## File Organization

**Pattern**: Type-based, client/server split

```text
client/             # Vue.js 3 frontend
  src/
    components/     # Reusable Vue components
    views/          # Page-level components
    services/       # API client and helpers
    assets/         # Static assets and styles
server/             # FastAPI backend
  routes/           # API route handlers
  models/           # Data models
  helpers/          # Utility functions
```

## Testing Strategy
TBD — No test framework currently configured.

## Error Handling
TBD

## Logging
TBD
