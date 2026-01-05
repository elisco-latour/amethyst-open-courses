---
id: "py_ds_05"
title: "Understanding Dictionaries"
type: "reading"
xp: 30
---

# Understanding Dictionaries in Python

Dictionaries are one of Python's most powerful built-in data types. They store data in **key-value pairs**, making them perfect for representing real-world objects and relationships.

## Creating Dictionaries

You can create a dictionary using curly braces `{}` with key-value pairs:

```python
# Empty dictionary
empty_dict = {}
# or
empty_dict = dict()

# Dictionary with initial values
person = {
    "name": "Alice",
    "age": 30,
    "city": "New York"
}

# Using dict() constructor
person = dict(name="Alice", age=30, city="New York")
```

## Key Characteristics

<div class="key-concept">
<h4>🔑 Key Concept: Dictionary Properties</h4>

- **Unordered** (Python 3.7+: insertion order preserved)
- **Mutable**: You can change, add, and remove items
- **Keys must be unique**: Duplicate keys overwrite previous values
- **Keys must be immutable**: Strings, numbers, tuples (not lists!)
- **Values can be any type**: Including other dictionaries!

</div>

## Accessing Values

Access values using their keys:

```python
person = {"name": "Alice", "age": 30}

# Using square brackets
print(person["name"])     # Output: Alice

# Using get() - safer, returns None if key doesn't exist
print(person.get("name"))           # Output: Alice
print(person.get("email"))          # Output: None
print(person.get("email", "N/A"))   # Output: N/A (default value)
```

<div class="callout callout-warning">
⚠️ **Warning**: Using `dict["key"]` raises a `KeyError` if the key doesn't exist. Use `.get()` when you're unsure if a key exists.
</div>

## Common Dictionary Operations

### Adding and Updating

```python
person = {"name": "Alice"}

# Add new key-value pair
person["email"] = "alice@email.com"

# Update existing value
person["name"] = "Alice Smith"

# Update multiple at once
person.update({
    "age": 31,
    "city": "Boston"
})
```

### Removing Items

```python
person = {"name": "Alice", "age": 30, "city": "NYC"}

# Remove and return value
age = person.pop("age")          # Returns 30

# Remove last inserted item (Python 3.7+)
item = person.popitem()          # Returns ("city", "NYC")

# Delete key
del person["name"]

# Clear all items
person.clear()
```

### Iterating Over Dictionaries

```python
person = {"name": "Alice", "age": 30, "city": "NYC"}

# Iterate over keys (default)
for key in person:
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")
```

## Dictionary Comprehensions

Just like lists, dictionaries support comprehensions:

```python
# Create a dict of squares
squares = {x: x**2 for x in range(5)}
# Result: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Filter and transform
numbers = {"a": 1, "b": 2, "c": 3, "d": 4}
even = {k: v for k, v in numbers.items() if v % 2 == 0}
# Result: {"b": 2, "d": 4}
```

## Nested Dictionaries

Dictionaries can contain other dictionaries:

```python
users = {
    "user1": {
        "name": "Alice",
        "email": "alice@email.com"
    },
    "user2": {
        "name": "Bob",
        "email": "bob@email.com"
    }
}

# Access nested values
print(users["user1"]["name"])  # Output: Alice
```

<div class="callout callout-info">
📊 **Data Analysis Tip**: Dictionaries are the foundation of JSON data format, which you'll encounter frequently when working with APIs and data files!
</div>

## Summary

Dictionaries are essential for organizing data with meaningful keys instead of numeric indices. They're used extensively in:

- Configuration settings
- API responses (JSON)
- Database records
- Counting and grouping data

Master dictionaries, and you'll be well-prepared for data manipulation in Python!
