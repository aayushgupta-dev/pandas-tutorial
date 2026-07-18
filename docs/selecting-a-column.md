# Selecting a column

Accessing a single column off a DataFrame — `df['Country']` — gives back a **Series**, not a plain Python list.

That Series carries the same row index as the DataFrame it came from — it's the same "DataFrame is a collection of Series sharing an index" idea from [dataframes-and-series.md](dataframes-and-series.md), just seen from the other direction: pull one column out, and you get one of those Series back.

## Syntax

- `df['Country']` — bracket notation, column name as a string. Always works, the safe default.
- `df.Country` — attribute (dot) notation, no quotes needed. Works too, but only if the column name is a valid Python identifier (no spaces) and doesn't collide with an existing DataFrame method/attribute name (a column named `count` or `index`, for instance, would shadow the real method).
- `df.[Country]` — **not valid syntax.** Don't mix bracket access with an unquoted name.
