---
id: "py_pandas_01_repo"
title: "Pandas Series repo"
type: "coding"
xp: 500
---

# Pandas Series
Import pandas as pd and create a Series `s` from the list `[10, 20, 30]`.

<!-- SEPARATOR -->

# seed_code
import pandas as pd

<!-- SEPARATOR -->

# validation_code
import pandas as pd
assert isinstance(s, pd.Series), "s should be a Series"
assert s.tolist() == [10, 20, 30], "s should contain [10, 20, 30]"
