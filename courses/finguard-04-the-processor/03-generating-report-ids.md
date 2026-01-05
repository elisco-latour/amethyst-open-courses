---
id: "finguard_04_03"
title: "Sequence Generation"
type: "coding"
xp: 100
---

# Sequence Generation

Audit trails require **Order**. A report needs a unique, sequential Identifier (ID) so auditors can check if a document is missing.

If we have Report #1 and Report #3, we know Report #2 is missing. This is crucial for **Data Integrity**.

## The `range()` Generator

We don't type lists of numbers manually (`[1, 2, 3...]`). We generate them.

```python
# range(start, stop)
# Note: 'stop' is EXCLUSIVE!
numbers = range(1, 6) # Generates: 1, 2, 3, 4, 5
```

## ID Formatting

Raw numbers aren't professional. We use **Zero-Padding** to maintain alignment.
*   `1` -> `001`
*   `99` -> `099`
*   `100` -> `100`

```python
seq_num = 5
print(f"INV-{seq_num:03d}") # Output: INV-005
```

`03d` means: "Format as a Decimal (Integer), pad with Zeros to 3 digits."

## Task

Generate a batch of **5 Report IDs** for the year 2025.
The format must be: `RPT-2025-001`, `RPT-2025-002`, etc.

1.  Use a `for` loop with `range()` to generate numbers 1 through 5.
2.  Format each ID string using the `year` and the loop variable.
3.  Append each ID to the `report_ids` list.

<!-- SEPARATOR -->

# seed_code
# Context
year: int = 2025
report_ids: list[str] = []

# ID Generation Loop



# Manifest
print("=== Generated Report IDs ===")
for report_id in report_ids:
    print(report_id)

<!-- SEPARATOR -->

# validation_code
expected_ids = ["RPT-2025-001", "RPT-2025-002", "RPT-2025-003", "RPT-2025-004", "RPT-2025-005"]
assert report_ids == expected_ids, f"report_ids should be {expected_ids}"
assert len(report_ids) == 5, "Should have 5 report IDs"
