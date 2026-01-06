---
id: "finguard_07_01"
title: "Persistence Layers"
type: "coding"
xp: 100
---

# Persistence Layers

So far, all your code has been transient. If the power goes out, your variables vanish.
Financial records must be **Persistent**. They must survive a system reboot.

## Volatility vs. Durability

| Layer | Medium | Speed | Volatility | Use Case |
|-------|--------|-------|------------|----------|
| **Memory** | RAM | ⚡ 100ns | Volatile | Active computation |
| **Storage** | SSD/HDD | 🐢 10ms | Durable | Long-term records |

(Note: SSDs are 100,000x slower than RAM. That is why we load data into variables to process it, then save it back.)

## The Bridge: `open()`

To move data from Disk to RAM, we open a "stream".

```python
# The Old Way (Manual Management) - DANGEROUS
stream = open("ledger.txt", "r")
data = stream.read()
# If code crashes here, the file stays locked!
stream.close()
```

## The Safe Way: Context Managers

We use the `with` statement. It guarantees the file is closed, even if errors occur. It is the **Cleanup Protocol** (finally) built-in.

```python
# The Professional Way
with open("ledger.txt", "r") as stream:
    data = stream.read()
    print("Reading data...")
# Stream is auto-closed here, guaranteed.
```

## File Modes (The Access Control)

| Mode | Name | Behavior |
|------|------|----------|
| `"r"` | Read | Errors if missing. Default. |
| `"w"` | Write | **Destroys** existing content. Creates if new. |
| `"a"` | Append | Adds to the end. Safe for logs. |

## Task

Create a `save_backup` function.
1.  Accept `filename` and `content`.
2.  Use `with open(..., "w")` to write the content.
3.  Ensure it overwrites old backups.

<!-- SEPARATOR -->

# seed_code
def save_backup(filename: str, content: str) -> None:
    """
    Saves content to a file, overwriting if exists.
    """
    # Use context manager
    pass

# Integration
save_backup("backup_001.txt", "Account: 12345\nBalance: 500")
with open("backup_001.txt", "r") as check:
    print(check.read())

<!-- SEPARATOR -->

# validation_code
import os

test_file = "test_backup.txt"
test_content = "Critical Data"

if os.path.exists(test_file):
    os.remove(test_file)

save_backup(test_file, test_content)

assert os.path.exists(test_file), "File should be created"
with open(test_file, "r") as f:
    assert f.read() == test_content, "Content should match"

# Cleanup
os.remove(test_file)
if os.path.exists("backup_001.txt"):
    os.remove("backup_001.txt")

print("Validation passed!")

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
