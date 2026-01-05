---
id: "py_pandas_02_repo"
title: "Pandas DataFrame repo"
type: "coding"
xp: 500
---

# Pandas DataFrame
Create a DataFrame `df` with a column "A" containing `[1, 2, 3]`.

<!-- SEPARATOR -->

# seed_code
import pandas as pd
data = {}
df = pd.DataFrame(data)

<!-- SEPARATOR -->

# validation_code
assert "A" in df.columns, "Column A should exist"
assert df["A"].tolist() == [1, 2, 3], "Column A should contain [1, 2, 3]"
