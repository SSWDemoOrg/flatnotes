# Data Stack

## Overview
Flatnotes uses a database-less architecture — notes are stored as plain markdown files on disk, with Whoosh providing full-text search indexing.

## Database
None (flat file storage)

Notes are stored as `.md` files in a configurable directory. This is a core design decision — flatnotes is intentionally database-free, making it simple to back up (copy a folder), migrate, and self-host. No database server to manage or configure.

## Search Index
Whoosh (pure Python search engine library)

Whoosh builds and maintains a full-text search index over the markdown files. It runs in-process with no external dependencies, keeping the self-hosted deployment simple.

## ORM / Database Client
Not applicable — file I/O via Python's standard library.

## Decision Relationships
- The database-less design is a core product differentiator, not a shortcut — it aligns with the self-hosted, zero-dependency philosophy
- Whoosh was chosen as a pure-Python solution to avoid native dependencies (e.g., Elasticsearch) that would complicate Docker builds and deployment
- File-based storage means backup/restore is simply copying a directory
