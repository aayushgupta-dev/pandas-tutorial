# Finding the project root

**Script:** [`scripts/pandas-basics.ipynb`](../scripts/pandas-basics.ipynb)

Relative paths (`"data/orders.csv"`) only work if you happen to run the code from the repo root. The moment you run it from a notebook, a nested script, or an IDE that launches from somewhere else, the relative path breaks. The fix: don't hardcode a relative path — find the root directory at runtime and build the path from there.

## The technique

Walk upward from the current directory through its ancestors, checking each one for a marker that only exists at the root, and stop at the first match:

```python
from pathlib import Path

current = Path.cwd()
root = next(p for p in [current, *current.parents] if (p / ".git").exists())
```

`current.parents` yields every ancestor directory, nearest first, terminating at the filesystem root. `[current, *current.parents]` puts the current directory itself first in that list, so `next(...)` returns the closest directory (at or above the current one) containing a `.git` folder — i.e. the repo root.

## Notes

- In a notebook, `Path.cwd()` is the right call since there's no `__file__` to anchor to. In a plain `.py` script, prefer `Path(__file__).resolve()` — it's tied to the file itself, not wherever the process happened to be launched from.
- `.git` is a solid marker for a local checkout, but it won't exist if the code ever ships without its git history (a zipped release, a built package). `pyproject.toml` is the more portable marker for a Python project specifically, since it travels with the source regardless of `.git`.
- If the marker is never found walking all the way up, `next()` raises `StopIteration` — not a very informative error on its own. Fine for a personal script; production code would pass `next(..., default=None)` and raise a clearer error when `None`.
