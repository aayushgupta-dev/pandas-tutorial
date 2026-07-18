# Loading data

**Script:** [`scripts/load_data.py`](../scripts/load_data.py)

`pd.read_csv()` reads a CSV straight into a `DataFrame`, inferring column names (from the header row) and dtypes (from the values) in one pass. That inference is convenient but not free — it's the first place real pipelines get bitten:

- A column read as `object` (string) when it should be numeric usually means a stray non-numeric value somewhere in that column (e.g. `"N/A"`, a stray comma). Check with `df.dtypes` before trusting downstream math.
- Dates are **not** parsed automatically — `OrderDate` here loads as a string. Use `parse_dates=["OrderDate"]` or `pd.to_datetime()` after loading if you need date arithmetic.
- For large files, `read_csv` loads the whole thing into memory at once. Production code reaching for pandas on multi-GB inputs typically uses `chunksize=` to stream, or reaches for a columnar format (Parquet) instead of CSV.

## Dataset

`data/orders.csv` — 40 rows of order data (customer, product, category, quantity, price, order date, shipped, country), sourced from the [companion repo](https://github.com/techwithtim/PanadasTutorial) for Tech With Tim's pandas tutorial. Treat it as a read-only fixture — never overwrite it; write any derived output (e.g. `cleaned_orders.csv`) to a new file.
