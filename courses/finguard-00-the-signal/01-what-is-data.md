---
id: "finguard_00_01"
title: "The Raw Material: Signal vs. Noise"
type: "coding"
xp: 100
---

# The Raw Material

You now know the mission: build FinGuard, a fraud detection system that protects real money. But before you can detect fraud, you need to understand the raw material you'll be working with — **data**.

## The Bank Vault Analogy

Imagine you're standing in a bank vault. Around you are rows of safety deposit boxes. Each box has a **label** (a name) and **contents** (a value). 

This is exactly how computers store information. Every piece of data in FinGuard — account numbers, transaction amounts, timestamps — lives in a "box" with a label. In Python, we call these boxes **variables**.

## What is Data, Really?

When a customer transfers $1,500, something real happens. A physical obligation is created — money must move from one account to another. Your job as a FinGuard engineer is to create a digital record of that obligation.

But here's the challenge: **computers don't understand money**. They only understand two states:
- **High Voltage** = 1 (the switch is ON)
- **Low Voltage** = 0 (the switch is OFF)

Everything — every dollar amount, every customer name, every transaction — must be translated into patterns of 1s and 0s.

## Your First Encoding

Python makes this translation easy. When you write `1500`, Python converts it into binary (a pattern of switches) behind the scenes.

You can peek at this hidden representation using `bin()`:
```python
bin(1500)  # Returns '0b10111011100'
```

The `0b` prefix means "this is binary." The rest is the actual switch pattern.

## Task

A customer has deposited $2,750 into their account. Your task:
1. Create a variable called `deposit_amount` and assign it the value `2750`
2. Create a variable called `binary_representation` that stores the binary form of the deposit amount (use the `bin()` function)

<!-- SEPARATOR -->

# seed_code
# Record the deposit amount (in whole dollars)
deposit_amount = 

# Inspect how the computer actually stores this number
binary_representation = 

# Verify your work
print(f"Deposit Amount: ${deposit_amount}")
print(f"Stored As: {binary_representation}")

<!-- SEPARATOR -->

# validation_code
assert deposit_amount == 2750, "The deposit amount should be 2750"
assert binary_representation == bin(2750), "Use bin() to get the binary representation of deposit_amount"
