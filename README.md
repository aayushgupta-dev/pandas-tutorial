# pandas-tutorial

Learning pandas by coding along with Tech With Tim's tutorial.

- Video: https://www.youtube.com/watch?v=EXIgjIBu4EU
- Companion repo: https://github.com/techwithtim/PanadasTutorial

## Setup

```
uv sync
uv run python scripts/load_data.py
```

## Structure

- `scripts/` — one script per pandas concept, run directly with `uv run python scripts/<name>.py`.
- `docs/` — one markdown file per concept, in my own words after talking the concept through. Not every topic has a script — some are conceptual only.
- `data/` — read-only fixture datasets used by the scripts (`orders.csv`, from the companion repo). Never overwrite these; derived output goes to new files.

Full conventions: [docs/repo-structure.md](docs/repo-structure.md).

## Documentation

| Topic | Doc | Script |
|---|---|---|
| DataFrames and Series | [dataframes-and-series.md](docs/dataframes-and-series.md) | — |
| Loading data | [load-data.md](docs/load-data.md) | [load_data.py](scripts/load_data.py) |
