---
id: "finguard_07_05"
title: "Format Strategy"
type: "coding"
xp: 100
---

# Format Strategy

An architect doesn't use concrete for windows. An engineer doesn't use CSV for configs.

Choosing the format is the first step of **System Design**.

## The Decision Matrix

| Requirement | Format | Why? |
|-------------|--------|------|
| **bulk_data_ingest** | `CSV` | Compact, streamable, readable by Excel. |
| **api_response** | `JSON` | Structured, nested, web-standard. |
| **human_report** | `TXT` | Formatting freedom, no strict syntax. |
| **config_file** | `YAML/JSON` | Easy to edit, hierarchical. |

## The Data Pipeline

FinGuard data flows through transformations:

1.  **Ingest**: Read millions of rows from `CSV`.
2.  **Process**: Convert to Python Objects (`dicts/classes`).
3.  **Persist**: Save state to `Database` (or JSON for now).
4.  **Report**: Generate `TXT` summary for humans.

## Task

Complete the `get_format_extension` strategy function.
1.  Input: `use_case` (string).
2.  Logic:
    - "bulk" -> ".csv"
    - "api" -> ".json"
    - "report" -> ".txt"
    - Anything else -> ".dat"
3.  Return the extension string.

<!-- SEPARATOR -->

# seed_code
def get_format_extension(use_case: str) -> str:
    """
    Returns the file extension for a given use case.
    Strategies: bulk, api, report.
    """
    # Use match/case or if/elif
    pass

# Integration
print(get_format_extension("bulk"))   # .csv
print(get_format_extension("api"))    # .json

<!-- SEPARATOR -->

# validation_code
# Test known use cases
assert get_format_extension("bulk") == ".csv", "bulk data should use .csv"
assert get_format_extension("api") == ".json", "API should use .json"
assert get_format_extension("report") == ".txt", "reports should use .txt"

# Test fallback
assert get_format_extension("unknown") == ".dat", "unknown should default to .dat"
assert get_format_extension("random") == ".dat", "any unknown should default to .dat"
