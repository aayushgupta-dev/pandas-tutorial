# Repo structure

How this repository is organized, and the rules for keeping it that way as it grows. This is a process doc (how the repo is laid out), not a pandas concept doc — it isn't part of the concept table in the README.

## Layout

```
pandas-tutorial/
├── main.py                      # plain uv-init scaffold entry point — not lesson code, leave as-is
├── scripts/                      # code — .py files and/or notebooks, not required to be one-per-topic
│   ├── load_data.py                 # one topic, one .py file (fine when a concept is self-contained)
│   └── pandas-basics.ipynb          # shared notebook — multiple topics/cells accumulate here
├── docs/                          # one markdown file per topic discussed, code or not
│   ├── repo-structure.md            # this file
│   ├── dataframes-and-series.md     # no code — purely conceptual topic
│   ├── load-data.md                 # backed by scripts/load_data.py
│   └── finding-project-root.md      # backed by scripts/pandas-basics.ipynb
├── data/                           # read-only fixture datasets
│   └── orders.csv
├── pyproject.toml / uv.lock / .python-version
└── README.md
```

## Rules

**`scripts/` — code files, `.py` or `.ipynb`, not required to be one-per-topic.**
The original rule was one `.py` file per topic. In practice, code now mostly accumulates in a single shared Jupyter notebook (`pandas-basics.ipynb`) — multiple concepts live there as separate cells rather than each getting its own file. A standalone `.py` file (like `load_data.py`) is still the right call when a concept is cleanly self-contained. Either way: **one code file can back multiple `docs/*.md` entries.** Docs stay one-per-concept regardless of how the code underneath is organized. Keep whichever file near-comment-free either way — comments only for something genuinely non-obvious; long explanations belong in `docs/`.

**`docs/` — one file per topic discussed, code or not.**
Every topic still gets its own `docs/<topic-in-kebab-case>.md`, written right after the user's explanation is validated — it doesn't wait on code existing. Name it after the topic directly (e.g. `dataframes-and-series.md`, `finding-project-root.md`); when a topic has its own dedicated `.py` script, the doc filename may mirror it (script's stem, underscores swapped for hyphens: `load_data.py` → `load-data.md`), but that's a convenience, not a requirement — several docs can equally point back to the same shared notebook.

These docs are **the user's own notes, in their own words** — not Claude-authored technical documentation. The flow (see CLAUDE.md's "Learning flow" section): the user explains the concept back after watching the video, Claude validates/corrects that explanation, the doc gets written from that validated explanation — simple, plain-language, "here's what this is and why," not formal reference prose. If a code component exists or gets added later, the doc links back to whichever file backs it (a dedicated script or the shared notebook) and notes the production-grade equivalent where the tutorial's version is intentionally simplified (per CLAUDE.md's "not just a tutorial-follow-along" section). If the user's understanding of a topic changes later, the doc gets updated in place rather than left stale.

**`data/` — read-only fixtures only.**
Scripts read from `data/`, never write into it. Any derived output (a cleaned/filtered/aggregated CSV, etc.) goes to a new file elsewhere, never back into `data/`. `.gitignore` enforces this: everything under `data/` is ignored except the specific fixture files explicitly un-ignored (`!data/orders.csv`). If you add a new fixture, whitelist it the same way.

**`main.py` stays inert.**
It's the placeholder `uv init` created. Lesson code lives in `scripts/`, not here — this keeps the entry point stable and avoids one file silently accumulating every lesson's code.

**`README.md` stays a high-level index.**
Setup instructions plus the docs table (topic → doc → script). Update that table whenever a new `docs/` file is added. Long-form explanation belongs in `docs/`, not README.

## Why this shape

The failure mode this avoids: a tutorial repo where every lesson's code gets appended to (or overwrites) a single `main.py`, comments pile up explaining half-remembered concepts inline, and there's no separate place for "why" vs "what." Splitting **scripts** (what/how — runnable, minimal comments) from **docs** (why — explanation, links, production notes) from **data** (fixtures, read-only) keeps each concern in exactly one place, so the repo stays navigable as more lessons get added instead of turning into an undifferentiated pile of files in the root.

Decoupling `docs/` from any particular code-file layout (one script per topic vs. one shared notebook) means how the code happens to be organized on a given day never forces a docs reorg — the concept notes stay one-per-topic no matter how the underlying code shuffles around.
