# pandas-tutorial

Learning pandas by coding along with Tech With Tim's tutorial.

- Video: https://www.youtube.com/watch?v=EXIgjIBu4EU
- Companion repo: https://github.com/techwithtim/PanadasTutorial

## Setup

```
uv sync
uv run jupyter lab   # work through scripts/pandas-basics.ipynb
```

## Structure

- `scripts/` — code: mostly `scripts/pandas-basics.ipynb`, a shared notebook where new concepts accumulate as cells, plus the occasional standalone `.py` file for a self-contained concept.
- `docs/` — one markdown file per concept, in my own words after talking the concept through. Not every topic has code behind it — some are conceptual only — and several docs can point back to the same notebook.
- `data/` — read-only fixture datasets used by the scripts (`orders.csv`, from the companion repo). Never overwrite these; derived output goes to new files.

Full conventions: [docs/repo-structure.md](docs/repo-structure.md).

## Documentation

| Topic | Doc | Script |
|---|---|---|
| DataFrames and Series | [dataframes-and-series.md](docs/dataframes-and-series.md) | — |
| Loading data | [load-data.md](docs/load-data.md) | [load_data.py](scripts/load_data.py) |
| Finding the project root | [finding-project-root.md](docs/finding-project-root.md) | [pandas-basics.ipynb](scripts/pandas-basics.ipynb) |
| Exploring a DataFrame | [exploring-a-dataframe.md](docs/exploring-a-dataframe.md) | [pandas-basics.ipynb](scripts/pandas-basics.ipynb) |
| Selecting a column | [selecting-a-column.md](docs/selecting-a-column.md) | — |
