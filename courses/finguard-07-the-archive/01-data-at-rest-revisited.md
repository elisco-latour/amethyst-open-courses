---
id: "finguard_07_01"
title: "Data at Rest (Revisited)"
type: "coding"
xp: 100
---

# Data at Rest (Revisited)

## The Bridge: From Memory to Disk

In **The Signal**, you learned that data can be "at rest" (stored) or "in motion" (flowing). Now you'll work with **data at rest** directly — files on disk.

## Why Files Matter

| Storage Type | Speed | Persistence | Use Case |
|--------------|-------|-------------|----------|
| Variables (RAM) | ⚡ Fastest | ❌ Lost on restart | Active computation |
| Files (Disk) | 🐢 Slower | ✅ Survives restart | Long-term storage |
| Database | 🚗 Medium | ✅ Survives restart | Structured queries |

## Opening Files

Python uses the `open()` function:

```python
# Open for reading
file = open("transactions.txt", "r")  # "r" = read mode
content = file.read()
file.close()  # Always close when done!
```

## The `with` Statement (Context Manager)

**Never manually close files.** Use `with` — it closes automatically:

```python
with open("transactions.txt", "r") as file:
    content = file.read()
# File is automatically closed here
```

## File Modes

| Mode | Meaning |
|------|---------|
| `"r"` | Read (file must exist) |
| `"w"` | Write (creates/overwrites) |
| `"a"` | Append (adds to end) |
| `"x"` | Exclusive create (fails if exists) |

## The Analogy: The Filing Cabinet

- **Opening a file** = Opening a drawer
- **Reading** = Looking at documents
- **Writing** = Putting new documents in
- **Closing** = Shutting the drawer

If you leave the drawer open (forget to close), someone might trip over it (resource leak).

## The "Pro" Tip

> **Always use `with open()`. It guarantees the file closes even if an error occurs.**

## Task

Read the contents of a transaction log and count the lines.

<!-- SEPARATOR -->

# seed_code
# Simulate a transaction log file
log_content: str = """2025-01-15 09:00:00 TXN-001 DEPOSIT $500.00
2025-01-15 09:15:00 TXN-002 WITHDRAWAL $200.00
2025-01-15 10:30:00 TXN-003 TRANSFER $1500.00
2025-01-15 11:00:00 TXN-004 DEPOSIT $3000.00
2025-01-15 14:45:00 TXN-005 WITHDRAWAL $750.00"""

# In a real app, you'd read from a file:
# with open("transactions.log", "r") as file:
#     log_content = file.read()

# Process the log content
lines: list[str] = log_content.strip().split("\n")
line_count: int = 0
deposit_count: int = 0
withdrawal_count: int = 0

# Count lines and transaction types
for line in lines:
    pass  # Replace with your implementation


print("=== Transaction Log Analysis ===")
print(f"Total entries: {line_count}")
print(f"Deposits: {deposit_count}")
print(f"Withdrawals: {withdrawal_count}")

<!-- SEPARATOR -->

# validation_code
assert line_count == 5, "Should have 5 log entries"
assert deposit_count == 2, "Should have 2 deposits"
assert withdrawal_count == 2, "Should have 2 withdrawals"
