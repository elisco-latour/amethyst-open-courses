---
id: "de_py_foundation_03_repo"
title: "The Engine repo"
type: "coding"
xp: 100
---

# The Engine
A single log isn't enough to test a pipeline. We need a stream of data.
We can use a **Loop** to repeat an action multiple times.

## Task
Use a `for` loop to append the string `"Heartbeat"` to the `events` list 5 times.

<!-- SEPARATOR -->

# seed_code
events = []

# Write a for loop here to add "Heartbeat" 5 times


<!-- SEPARATOR -->

# validation_code
assert len(events) == 5, "events list should have 5 items"
assert all(e == "Heartbeat" for e in events), "All events should be 'Heartbeat'"
