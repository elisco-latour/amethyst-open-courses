---
id: "de_py_foundation_05_repo"
title: "The Model repo"
type: "coding"
xp: 100
---

# The Model
As our system grows, using plain dictionaries can lead to errors (e.g., typos in keys).
We need a strict blueprint for our data.
**Classes** allow us to define custom data types with their own behaviors.

## Task
Define a class named `LogEvent`.
1. The `__init__` method should take `level` and `message` and store them as instance attributes.
2. Add a method `to_dict(self)` that returns the data as a dictionary.

<!-- SEPARATOR -->

# seed_code
class LogEvent:
    def __init__(self, level, message):
        # Initialize attributes
        pass

    def to_dict(self):
        # Return dictionary representation
        pass

<!-- SEPARATOR -->

# validation_code
event = LogEvent("WARN", "Low memory")
assert event.level == "WARN", "Attribute level not set"
assert event.message == "Low memory", "Attribute message not set"
assert event.to_dict() == {"level": "WARN", "message": "Low memory"}, "to_dict method failed"
