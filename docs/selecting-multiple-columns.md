# Selecting multiple columns

**Script:** [`scripts/pandas-basics.ipynb`](../scripts/pandas-basics.ipynb)

Passing a **list** of column names in brackets — `df[["Country", "Product", "CustomerName"]]` (note the double brackets) — returns a **DataFrame**, not a Series: a subset of the original with just those columns, all rows.

This is the same `[]` operator as [selecting a single column](selecting-a-column.md), just with a list instead of a single string as the argument — and that's exactly what flips the return type:

- `df["Country"]` → single string → **Series**
- `df[["Country", "Product"]]` → list of strings → **DataFrame**

## Careful with mutation

Don't treat the result as a safe, mutable "view" of the original DataFrame. If you plan to modify the subset afterward, call `.copy()` on it explicitly (`sub = df[[...]].copy()`) — otherwise you're in the same ambiguous territory as chained indexing, where pandas can't guarantee whether you're looking at a view or a copy, and raises `SettingWithCopyWarning` when it isn't sure.
