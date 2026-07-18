# Selecting a column

**Script:** [`scripts/pandas-basics.ipynb`](../scripts/pandas-basics.ipynb)

Accessing a single column off a DataFrame — `df['Country']` — gives back a **Series**, not a plain Python list.

That Series carries the same row index as the DataFrame it came from — it's the same "DataFrame is a collection of Series sharing an index" idea from [dataframes-and-series.md](dataframes-and-series.md), just seen from the other direction: pull one column out, and you get one of those Series back.

## Syntax

- `df['Country']` — bracket notation, column name as a string. Always works, the safe default.
- `df.Country` — attribute (dot) notation, no quotes needed. Works too, but only if the column name is a valid Python identifier (no spaces) and doesn't collide with an existing DataFrame method/attribute name (a column named `count` or `index`, for instance, would shadow the real method).
- `df.[Country]` — **not valid syntax.** Don't mix bracket access with an unquoted name.

## Working with the resulting Series

Once you have `df["Country"]`, it behaves like any other Series:

- `len(df["Country"])` — number of elements (all rows, including duplicates — and any `NaN`s count too).
- `df["Country"].unique()` — the pandas-native way to get distinct values. Returns a numpy array, **preserves first-seen order**.
- `df["Country"].nunique()` — just the count of distinct values.
- `set(df["Country"])` — Python's built-in `set()` also works and gets you distinct values, but it drops order and isn't the idiomatic pandas approach — prefer `.unique()`/`.nunique()` above.

Gotcha specific to notebooks: in a Jupyter cell, only the **last** expression auto-displays. `df["Country"]` or `len(df["Country"])` sitting mid-cell as a bare expression (not the last line, not wrapped in `print()`) silently computes and shows nothing.
