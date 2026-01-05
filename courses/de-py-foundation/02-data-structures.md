---
id: "de_py_foundation_02_repo"
title: "The Data Packet repo"
type: "coding"
xp: 100
---

# The Data Packet
Great, we have our app configuration. Now we need to structure the data we are going to log.
A single log entry contains multiple pieces of information: a timestamp, a severity level, and a message.

In Python, a **Dictionary** is perfect for holding related data with named keys.
A **List** is used to hold an ordered collection of items (like a history of logs).

## Task
1. Create a dictionary named `log_entry` with the keys:
   - `"timestamp"`: `"2023-10-27 10:00:00"`
   - `"level"`: `"INFO"`
   - `"message"`: `"System initialized"`
2. Create a list named `log_history` containing just this one `log_entry`.

<!-- SEPARATOR -->

# seed_code
# Create the log_entry dictionary


# Create the log_history list


<!-- SEPARATOR -->

# validation_code
assert isinstance(log_entry, dict), "log_entry must be a dictionary"
assert log_entry["level"] == "INFO", "log_entry level should be 'INFO'"
assert isinstance(log_history, list), "log_history must be a list"
assert log_history[0] == log_entry, "log_history should contain the log_entry"
