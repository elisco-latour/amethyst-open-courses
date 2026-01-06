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
assert get_format_extension("bulk") == ".csv"
assert get_format_extension("api") == ".json"
assert get_format_extension("report") == ".txt"
assert get_format_extension("unknown") == ".dat"
print("Validation passed!")
    reader = csv.DictReader(StringIO(csv_content))
    return list(reader)
```

## The "Pro" Tip

> **Let the consumer choose the format. APIs often support `?format=json` or `?format=csv`.**

## Task

Build a multi-format exporter that can output transaction data in CSV, JSON, or plain text report format.

<!-- SEPARATOR -->

# seed_code
import csv
import json
from decimal import Decimal
from io import StringIO

# Transaction data to export
transactions: list[dict] = [
    {"id": "TXN-001", "amount": "500.00", "type": "DEPOSIT"},
    {"id": "TXN-002", "amount": "200.00", "type": "WITHDRAWAL"},
    {"id": "TXN-003", "amount": "1500.00", "type": "TRANSFER"},
]

def export_csv(data: list[dict]) -> str:
    """Export transactions to CSV format."""
    pass  # Replace with your implementation


def export_json(data: list[dict]) -> str:
    """Export transactions to JSON format."""
    pass  # Replace with your implementation


def export_report(data: list[dict]) -> str:
    """Export transactions to human-readable report."""
    pass  # Replace with your implementation


def export_transactions(data: list[dict], format: str) -> str:
    """Export transactions in the requested format.
    
    Args:
        data: List of transaction dicts
        format: One of 'csv', 'json', 'report'
    
    Returns:
        Formatted string
    """
    pass  # Replace with your implementation


# Test all formats
print("=== CSV Format ===")
print(export_transactions(transactions, "csv"))

print("\n=== JSON Format ===")
print(export_transactions(transactions, "json"))

print("\n=== Report Format ===")
print(export_transactions(transactions, "report"))

<!-- SEPARATOR -->

# validation_code
import json

# Test CSV export
csv_output = export_transactions(transactions, "csv")
assert "id,amount,type" in csv_output or "id" in csv_output.split("\n")[0], "CSV should have header row"
assert "TXN-001" in csv_output, "CSV should contain transaction ID"
assert "500.00" in csv_output, "CSV should contain amount"

# Test JSON export  
json_output = export_transactions(transactions, "json")
parsed = json.loads(json_output)
assert isinstance(parsed, list), "JSON should be a list"
assert len(parsed) == 3, "JSON should have 3 transactions"
assert parsed[0]["id"] == "TXN-001", "JSON should preserve transaction ID"

# Test report export
report_output = export_transactions(transactions, "report")
assert "TXN-001" in report_output or "3" in report_output, "Report should contain data"
assert len(report_output) > 50, "Report should be human-readable (not just raw data)"
