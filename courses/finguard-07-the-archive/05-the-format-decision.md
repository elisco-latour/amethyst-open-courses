---
id: "finguard_07_05"
title: "The Format Decision"
type: "coding"
xp: 100
---

# The Format Decision

Real systems use multiple formats. Knowing **when** to use each is as important as knowing **how**.

## Format Selection Guide

| Scenario | Best Format | Why |
|----------|-------------|-----|
| Spreadsheet export | CSV | Excel-compatible |
| API communication | JSON | Nested data, types |
| Configuration files | JSON/YAML | Human-editable |
| Log files | Plain text | Append-friendly |
| Batch data exchange | CSV | Simple, universal |
| Report for humans | Plain text | Readable |

## The Bridge: Data Lifecycle

```
Data Entry → [JSON API] → Processing → [CSV Export] → Archive → [Text Logs]
```

Different stages of the data lifecycle need different formats.

## Converting Between Formats

```python
import csv
import json
from io import StringIO

# JSON → CSV (flattening nested data)
def json_to_csv(json_data: list[dict]) -> str:
    if not json_data:
        return ""
    
    output = StringIO()
    writer = csv.DictWriter(output, fieldnames=json_data[0].keys())
    writer.writeheader()
    writer.writerows(json_data)
    return output.getvalue()

# CSV → JSON
def csv_to_json(csv_content: str) -> list[dict]:
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
