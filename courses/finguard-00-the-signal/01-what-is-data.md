---
id: "finguard_00_01"
title: "What is Data?"
type: "coding"
xp: 100
---

# What is Data?

Welcome to FinGuard. Before you write a single line of code, you need to understand the raw material you'll be working with: **Data**.

## The Analogy: The Bank Vault

Imagine you're standing in a bank vault. Around you are thousands of safety deposit boxes. Each box contains something valuable: contracts, certificates, deeds, photographs.

But here's the thing — the **paper** isn't valuable. The **information ON the paper** is valuable.

**Data is information encoded in a format that machines can process.**

That wedding certificate? To a computer, it's just:
- Characters: `"Marriage Certificate"`
- A date: `1995-06-15`
- Two names: `"Alice Smith"`, `"Bob Jones"`

The computer doesn't understand "love" or "commitment." It only sees **symbols** that follow **rules**.

## The Building Blocks

Everything in a computer comes down to **electricity**: ON or OFF, 1 or 0.

```
1 bit    = 0 or 1                     (one switch)
8 bits   = 1 byte                     (one character, like "A")
1,000 bytes = 1 kilobyte (KB)         (a short email)
1,000,000 bytes = 1 megabyte (MB)     (a photo)
1,000,000,000 bytes = 1 gigabyte (GB) (a movie)
```

When FinGuard processes a transaction, it's really just manipulating billions of tiny switches.

## Your First Data

Let's see this in action. The code below represents a single transaction as a **number**.

The transaction amount `1500.00` is stored as bits. Python hides this complexity, but it's happening.

## Task

Run the code to see how Python represents the number `1500` in binary (1s and 0s).

This is what the computer actually "sees."

<!-- SEPARATOR -->

# seed_code
# A transaction amount in dollars
transaction_amount = 1500

# Let's see what the computer actually sees
binary_representation = bin(transaction_amount)

print(f"Human sees: ${transaction_amount}")
print(f"Computer sees: {binary_representation}")

<!-- SEPARATOR -->

# validation_code
assert transaction_amount == 1500, "Keep the transaction amount as 1500"
assert "0b" in binary_representation, "binary_representation should contain binary format"
