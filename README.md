# pandas_numba

A tiny numba `jitclass` holding pandas-style typed columns for use inside `@njit`
nopython code — plus a pandas DataFrame bridge and objmode glue to call back into
pandas from jitted code.

- **`Pandas_nb`** — a `jitclass` DataFrame-like container: one numpy array per
  column, keyed by title, one typed dict per element type.
- **`Pandas_tools`** — the pandas bridge: `DataFrame → Pandas_nb` and back, with
  columns shared as views where possible.
- **`f_eval_expr` + objmode** — carry a DataFrame by handle and run pandas on the
  live frame from inside `@njit`.

## Requirements

- Python 3.12
- `numpy`, `pandas`, `numba`

## Quick start

```python
import pandas as pd
from pandas_numba import Pandas_tools

df = pd.DataFrame({"x": [1.0, 2.0, 3.0], "n": [10, 20, 30]})
nb = Pandas_tools.f_df_to_nb(df)   # DataFrame -> Pandas_nb (numeric columns shared as views)
# ... use `nb` inside your own @njit functions ...
out = Pandas_tools.f_nb_to_df(nb)  # Pandas_nb -> DataFrame
```

## Documentation

See [`pandas_numba.md`](pandas_numba.md) for the full data model, API, the
pandas round-trip rules, the df-by-handle / string-eval objmode mechanism, and
the numba constraints.

## Tests

```
python -m pytest pandas_numba_tests.py -v
```

The suite is driven by one `ALL_TYPES` table — add a column type by adding one
row and every per-type test covers it.

## License

[MIT](LICENSE) © 2026 Damien Loison
