# Exploring a DataFrame

**Script:** [`scripts/pandas-basics.ipynb`](../scripts/pandas-basics.ipynb)

Once a DataFrame is loaded, these are the go-to methods for getting a first look at it before doing anything else with the data.

- **`df.head()`** — first 5 rows.
- **`df.tail()`** — last 5 rows.
- **`df.info()`** — the full structural summary: row/column counts, **non-null count per column** (the fastest way to spot missing data), dtypes, and memory usage. Dtypes are only one part of what it reports.
- **`df.describe()`** — summary statistics (count, mean, std, min, quartiles, max) for the numeric columns.
- **`df.columns`** — the column labels. This comes back as pandas' own `Index` object, not a plain Python list — it prints list-*like*, but if you need an actual `list`, call `.tolist()` on it.
- **`df.index`** — the row labels (here, the default `RangeIndex` pandas assigns when the CSV doesn't specify one).

## Gotcha: `print(df.info())`

`.info()` prints its summary directly and returns `None`. Wrapping it in `print()` — `print(df.info())` — prints the summary, then prints `None` right after on its own line. Same deal for `.head()`/`.tail()`/`.describe()` in a notebook, actually: in a Jupyter cell the last expression auto-displays without needing `print()` at all — `print()` is really only necessary here because these calls aren't the last line of the cell.
