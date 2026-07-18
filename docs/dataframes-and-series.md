# DataFrames and Series

*No script yet — this one's conceptual so far.*

## What is pandas?

Pandas is a Python library for data analysis. It reads and writes a bunch of formats, but CSV and Excel are the two you'll hit constantly.

## The two core data structures

- **Series** — a 1D labeled array. Think: a single column.
- **DataFrame** — a 2D labeled data structure. Think: a full spreadsheet or SQL table.

A DataFrame is really a collection of Series that all share the same row index — that's why pulling a single column out of a DataFrame (`df['col']`) hands you back something that behaves like its own little labeled array.

## Key features of a DataFrame

1. **Label-based indexing** — you can access rows/columns by their labels, not just position. In practice this splits into two tools: `.loc[]` (label-based) and `.iloc[]` (position-based).
2. **Fast** — built on top of NumPy, so operations are vectorized instead of looping row by row.
3. **Mixed data types** — different columns can hold different dtypes (numbers, strings, dates, etc.) within the same DataFrame.
4. **Row- and column-wise operations** — you can operate across a whole column or a whole row at once, not just cell by cell.

## Loading a DataFrame — the three common ways

```python
import pandas as pd

df = pd.read_csv("data/orders.csv")     # Load from CSV — our orders.csv fixture
df = pd.read_excel("data/orders.xlsx")  # Load from Excel — illustrative only, no .xlsx in this repo

data_dict = {
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35],
    "Country": ["USA", "Canada", "UK"],
}
df = pd.DataFrame(data_dict)            # Load from a dictionary
```

All three end up as a DataFrame — the source format doesn't matter once it's loaded. The dictionary case is the one worth remembering syntax-wise: keys become column names, each value list becomes that column's data.
