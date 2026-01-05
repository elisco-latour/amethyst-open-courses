---
id: "de_py_foundation_04"
title: "The Factory"
type: "coding"
xp: 100
---

# The Factory
Hardcoding log entries is inefficient. We need a reusable factory to create logs on demand.
In Python, **Functions** allow us to encapsulate logic and reuse it.

## Task
Define a function named `create_log` that takes two arguments: `level` and `message`.
It should return a dictionary with keys `"level"` and `"message"` set to the arguments provided.

<!-- SEPARATOR -->

# seed_code
def create_log(level, message):
    # Implement the function here
    pass

# Test your function
log = create_log("ERROR", "Connection failed")

<!-- SEPARATOR -->

# validation_code
test_log = create_log("DEBUG", "Test")
assert test_log["level"] == "DEBUG", "Function should set the level correctly"
assert test_log["message"] == "Test", "Function should set the message correctly"
