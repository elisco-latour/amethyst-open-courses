---
id: "finguard_04_03"
title: "Generating Report IDs"
type: "coding"
xp: 100
---

# Generating Report IDs

FinGuard generates daily reports with sequential IDs: `RPT-001`, `RPT-002`, etc. You need to generate a sequence of numbers.

## The `range()` Function

`range()` generates a sequence of numbers:

```python
# Generate 0, 1, 2, 3, 4
for i in range(5):
    print(i)

# Generate 1, 2, 3, 4, 5 (start, stop)
for i in range(1, 6):  # stop is exclusive!
    print(i)

# Generate 0, 2, 4, 6, 8 (start, stop, step)
for i in range(0, 10, 2):
    print(i)
```

## The Analogy: The Ticket Dispenser

At a bank, you take a number and wait. The dispenser doesn't have all numbers stored — it generates them on demand: 001, 002, 003...

`range()` works the same way. It doesn't create a list of millions of numbers. It generates them one at a time as needed.

## String Formatting for IDs

```python
# Zero-padded numbers
for i in range(1, 4):
    report_id = f"RPT-{i:03d}"  # :03d means 3 digits, zero-padded
    print(report_id)
# RPT-001
# RPT-002
# RPT-003
```

## The "Pro" Tip

> **`range(stop)` starts at 0, not 1. `range(1, n+1)` gives you 1 through n.**

```python
# ❌ Common mistake — starts at 0
for i in range(5):  # 0, 1, 2, 3, 4
    ...

# ✅ Start at 1 for human-friendly numbering
for i in range(1, 6):  # 1, 2, 3, 4, 5
    ...
```

## Task

Generate 5 report IDs for the daily compliance reports:
- `RPT-2025-001`
- `RPT-2025-002`
- `RPT-2025-003`
- `RPT-2025-004`
- `RPT-2025-005`

Store them in the `report_ids` list.

<!-- SEPARATOR -->

# seed_code
# Generate daily report IDs
year: int = 2025
report_ids: list[str] = []

# Generate 5 report IDs



# Print the generated IDs
print("=== Generated Report IDs ===")
for report_id in report_ids:
    print(report_id)

<!-- SEPARATOR -->

# validation_code
expected_ids = ["RPT-2025-001", "RPT-2025-002", "RPT-2025-003", "RPT-2025-004", "RPT-2025-005"]
assert report_ids == expected_ids, f"report_ids should be {expected_ids}"
assert len(report_ids) == 5, "Should have 5 report IDs"
