# tests-history/

Committed JSON snapshots of past test runs for each practice-test page.
When a practice-test page loads, it fetches the matching file here and
merges it into the browser's localStorage history — so your past tests
show up on any device the live site is opened on.

## Files

- `baseline-history.json` — history for `baseline_practice_test.html`
- `final-history.json` — history for `final_practice_test.html`
- `fourth-history.json` — history for `fourth_practice_test.html`

Each file is a JSON array of test objects (same shape used internally):

```json
[
  {
    "id": "T20261102-143005-ab12",
    "timestamp": 1762105805000,
    "questions": [ /* 25 {q, choices, answer, domain} */ ],
    "answers": [2, 1, 0, null, ...],
    "graded": true,
    "score": 21,
    "total": 25,
    "pct": 84
  }
]
```

## Workflow

1. **Take tests** in the browser as usual. Each test is saved to the browser's
   Past Tests history automatically.
2. **Export** — on any test page click **Export All**. A `<page>-history.json`
   file downloads to your computer.
3. **Commit** — drop that file into this folder with the matching name, then:
   ```
   git add tests-history/
   git commit -m "Update test history"
   git push
   ```
4. **Open on another device** — load the live site. The page reads the
   committed JSON on startup and merges it into Past Tests.

## Merge rules

When the browser merges local history with the committed file:

- Same test `id` → the record with the **newer timestamp** wins
- A **graded** record always wins over an **ungraded** one with the same id
- Everything else is added

So you can safely edit on multiple devices, export+commit from each, and
the newest version per test wins.

## Manual Import

The **Import** button on each test page lets you pick a `.json` file from
disk and merge it (useful if someone sends you theirs, or you want to
restore from a backup that isn't in this folder).

## Resetting

To wipe shared history for a page, commit an empty array (`[]`) to the
matching file, or delete the file entirely. Each browser's local history
is unaffected by that commit — only new tabs that reload the page will
pick up the change.
