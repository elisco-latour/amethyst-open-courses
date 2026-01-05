---
id: "py_pandas_03"
title: "Plotting with Pandas"
type: "coding"
xp: 600
---

# Plotting with Pandas
Create a simple bar plot using Pandas and Matplotlib.

Given the DataFrame `df` with columns `category` and `values`, create a bar plot where:
- The x-axis shows the categories
- The y-axis shows the values
- Store the Axes object in a variable called `ax`

Use `df.plot(kind='bar', x='category', y='values')` to create the plot.

<!-- SEPARATOR -->

# seed_code
import pandas as pd
import matplotlib.pyplot as plt

# Sample data
df = pd.DataFrame({
    'category': ['A', 'B', 'C', 'D'],
    'values': [25, 40, 30, 55]
})

# Create a bar plot and store in 'ax'


# Show the plot
plt.show()

<!-- SEPARATOR -->

# validation_code
import pandas as pd
import matplotlib.pyplot as plt

# Check that ax exists and is an Axes object
assert 'ax' in dir(), "You need to create a variable called 'ax'"
from matplotlib.axes import Axes
assert isinstance(ax, Axes), "ax should be a matplotlib Axes object"

# Check that it's a bar plot
assert len(ax.patches) == 4, "The plot should have 4 bars"
print("Great job! You created a bar plot with Pandas!")
