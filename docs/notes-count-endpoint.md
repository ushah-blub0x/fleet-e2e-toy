# Notes count endpoint

`GET /api/notes/count` returns `{ "count": <number> }`, the total number of notes in the in-memory store.

## Design

- Implemented in `src/api/notes.ts` alongside the other note handlers.
- Always responds 200 with `res.status(200).json(...)`; an empty store yields `{ "count": 0 }`.

## Invariant: route order

Express matches routes in registration order. `/count` MUST be registered before `/:id`, otherwise `count` is captured as a note id and the request returns 404 "Note not found". Any future static sub-path under `/api/notes` (e.g. `/stats`) needs the same placement.

## Testing

`tests/notes.test.ts` covers an empty store, count after creates, and count after creates plus a delete. The store is cleared before each case so tests are independent.
