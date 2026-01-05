---
id: "py_ds_03"
title: "Understanding Lists"
type: "reading"
xp: 30
---

# Understanding Lists in Python

Lists are one of the most versatile and commonly used data structures in Python. They allow you to store multiple items in a single variable and are **ordered**, **changeable**, and **allow duplicate values**.

## Creating Lists

You can create a list by placing comma-separated values inside square brackets `[]`:

```python
# Empty list
empty_list = []

# List with initial values
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]

# Mixed types (Python allows this!)
mixed = [1, "hello", 3.14, True]
```

## Key Characteristics

<div class="key-concept">
<h4>🔑 Key Concept: List Properties</h4>

- **Ordered**: Items have a defined order that won't change unless you explicitly modify it
- **Mutable**: You can change, add, and remove items after the list is created
- **Allow Duplicates**: Lists can contain items with the same value
- **Zero-indexed**: The first element is at index 0, not 1

</div>

## Accessing Elements

You can access list elements by their index:

```python
fruits = ["apple", "banana", "cherry"]

# Access by positive index
print(fruits[0])   # Output: apple
print(fruits[1])   # Output: banana

# Access by negative index (from the end)
print(fruits[-1])  # Output: cherry (last item)
print(fruits[-2])  # Output: banana (second to last)
```

## Common List Operations

### Adding Items

```python
fruits = ["apple", "banana"]

# Append to end
fruits.append("cherry")      # ["apple", "banana", "cherry"]

# Insert at specific position
fruits.insert(1, "orange")   # ["apple", "orange", "banana", "cherry"]

# Extend with another list
fruits.extend(["mango", "kiwi"])
```

### Removing Items

```python
fruits = ["apple", "banana", "cherry", "banana"]

# Remove by value (first occurrence)
fruits.remove("banana")      # ["apple", "cherry", "banana"]

# Remove by index
del fruits[0]               # Remove first item
popped = fruits.pop()       # Remove and return last item
popped = fruits.pop(1)      # Remove and return item at index 1
```

### List Slicing

Slicing allows you to get a portion of a list:

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Basic slicing [start:end]
print(numbers[2:5])    # [2, 3, 4]
print(numbers[:4])     # [0, 1, 2, 3]
print(numbers[6:])     # [6, 7, 8, 9]

# With step [start:end:step]
print(numbers[::2])    # [0, 2, 4, 6, 8] (every 2nd)
print(numbers[::-1])   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] (reversed)
```

<div class="callout callout-tip">
💡 **Pro Tip**: Use list comprehensions for concise list creation:

```python
squares = [x**2 for x in range(10)]
# Result: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```
</div>

## Summary

Lists are fundamental to Python programming. Understanding how to create, access, and manipulate lists will be essential as you progress through more advanced topics like data analysis with pandas.

In the next section, we'll test your knowledge with a quiz!
