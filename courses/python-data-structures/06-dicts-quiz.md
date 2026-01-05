---
id: "py_ds_06"
title: "Dictionaries Quiz"
type: "quiz"
xp: 50
---

# Dictionaries Quiz

Test your understanding of Python dictionaries!

<!-- SEPARATOR -->

# questions
- id: q1
  text: "Which of the following is a valid dictionary key in Python?"
  options:
    - id: a
      text: "[1, 2, 3]"
      isCorrect: false
    - id: b
      text: "(1, 2, 3)"
      isCorrect: true
    - id: c
      text: "{'nested': 'dict'}"
      isCorrect: false
    - id: d
      text: "All of the above"
      isCorrect: false
  explanation: "Dictionary keys must be immutable (hashable). Lists and dictionaries are mutable, so they can't be keys. Tuples are immutable and can be used as keys."

- id: q2
  text: "What happens when you access a non-existent key using `dict['key']`?"
  options:
    - id: a
      text: "Returns None"
      isCorrect: false
    - id: b
      text: "Returns an empty string"
      isCorrect: false
    - id: c
      text: "Raises KeyError"
      isCorrect: true
    - id: d
      text: "Creates the key with None value"
      isCorrect: false
  explanation: "Accessing a non-existent key with square brackets raises a KeyError. Use .get() method to safely access keys that might not exist."

- id: q3
  text: "What is the result of `len({'a': 1, 'b': 2, 'a': 3})`?"
  options:
    - id: a
      text: "2"
      isCorrect: true
    - id: b
      text: "3"
      isCorrect: false
    - id: c
      text: "Error"
      isCorrect: false
    - id: d
      text: "4"
      isCorrect: false
  explanation: "Dictionary keys must be unique. When you define 'a' twice, the second value (3) overwrites the first (1). The resulting dict is {'a': 3, 'b': 2} with length 2."

- id: q4
  text: "Which method returns a default value if a key doesn't exist?"
  options:
    - id: a
      text: "dict[key]"
      isCorrect: false
    - id: b
      text: "dict.get(key, default)"
      isCorrect: true
    - id: c
      text: "dict.find(key)"
      isCorrect: false
    - id: d
      text: "dict.default(key)"
      isCorrect: false
  explanation: "The .get(key, default) method returns the value if key exists, otherwise returns the default value (or None if no default is specified)."

- id: q5
  text: "What does `person.items()` return?"
  options:
    - id: a
      text: "A list of keys"
      isCorrect: false
    - id: b
      text: "A list of values"
      isCorrect: false
    - id: c
      text: "A view of key-value tuple pairs"
      isCorrect: true
    - id: d
      text: "The dictionary itself"
      isCorrect: false
  explanation: ".items() returns a dict_items view object containing (key, value) tuple pairs. Use .keys() for keys only and .values() for values only."
