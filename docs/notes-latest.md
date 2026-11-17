# GET /api/notes/latest

Returns the most recently created note, or 404 `{ "error": "No notes" }` when the store is empty.

## Design

- "Latest" is the note with the largest `createdAt`. `createdAt` is an ISO-8601 string, so plain string comparison orders correctly.
- Ties are broken by insertion order: the comparison is `>=`, so the note inserted last wins. The in-memory store is a `Map`, which preserves insertion order.
- No separate index is kept; the handler scans all notes. Acceptable for the intentional in-memory store.

## Invariant: route order

Static GET routes such as `/latest` must be declared before `/:id` in `src/api/notes.ts`. Otherwise Express matches `/:id` first and `latest` is treated as a note id (returning "Note not found"). Any future static sub-path needs the same placement.

## Testing notes

Tests use Jest fake timers with fixed system times so creation order is deterministic. Not yet covered: two notes sharing an identical `createdAt` (the tie rule).
