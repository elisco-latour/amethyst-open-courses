---
id: "finguard_01_01"
title: "The Variable: Labeling Reality"
type: "coding"
xp: 100
---

# The Variable: Labeling Reality

In **The Signal**, you learned that data is just bits. But you can't write software with `0b10101`.

You need to give those bits a **Name**.

## The Safety Deposit Box

Imagine the FinGuard Vault. It has millions of boxes.
If you put $5,000 in a box but don't label it, that money is gone.

In Python, a **Variable** is that label.

*   **The Box:** A specific location in your computer's memory.
*   **The Content:** The data (e.g., `1500`).
*   **The Label:** The variable name (e.g., `deposit_amount`).

## The First Rule of Engineering: Intent

Amateurs name variables `x`, `y`, or `temp`.
Engineers name variables to reveal their **Intent**.

> **"Code is read 10 times more often than it is written."**

If you name a variable `t`, your team has to guess what it means. If you name it `transaction_timestamp`, you have told the truth.

## Task

Create a variable that represents the **Transaction ID** of a wire transfer.
Use `snake_case` (all lowercase, underscores). This is the Python standard.

<!-- SEPARATOR -->

# seed_code
# Your first ledger entry.
# We need to label this specific transaction so we can trace it later.

# Create a variable named 'transaction_id' with value "TXN-2025-00001"
# 


# Print it to verify the label sticks
print(f"Transaction Recorded: {transaction_id}")

<!-- SEPARATOR -->

# validation_code
assert transaction_id == "TXN-2025-00001", "transaction_id should be 'TXN-2025-00001'"
assert isinstance(transaction_id, str), "transaction_id should be a string"
