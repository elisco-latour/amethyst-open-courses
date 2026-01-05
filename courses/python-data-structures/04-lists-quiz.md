---
id: "py_ds_04"
title: "Lists Quiz"
type: "quiz"
xp: 50
---

# Lists Quiz

Test your understanding of Python lists with this quiz!

<!-- SEPARATOR -->

# questions
- id: q1
  text: "What is the index of the first element in a Python list?"
  options:
    - id: a
      text: "1"
      isCorrect: false
    - id: b
      text: "0"
      isCorrect: true
    - id: c
      text: "-1"
      isCorrect: false
    - id: d
      text: "It depends on the list"
      isCorrect: false
  explanation: "Python uses zero-based indexing, so the first element is always at index 0."

- id: q2
  text: "Which method adds an element to the END of a list?"
  options:
    - id: a
      text: "insert()"
      isCorrect: false
    - id: b
      text: "add()"
      isCorrect: false
    - id: c
      text: "append()"
      isCorrect: true
    - id: d
      text: "extend()"
      isCorrect: false
  explanation: "append() adds a single element to the end of the list. insert() adds at a specific position, extend() adds multiple elements, and add() is not a list method."

- id: q3
  text: "What does `fruits[-1]` return if `fruits = ['apple', 'banana', 'cherry']`?"
  options:
    - id: a
      text: "apple"
      isCorrect: false
    - id: b
      text: "banana"
      isCorrect: false
    - id: c
      text: "cherry"
      isCorrect: true
    - id: d
      text: "Error"
      isCorrect: false
  explanation: "Negative indices count from the end of the list. -1 refers to the last element, -2 to the second-to-last, and so on."

- id: q4
  text: "What is the result of `[1, 2, 3] + [4, 5]`?"
  options:
    - id: a
      text: "[5, 7]"
      isCorrect: false
    - id: b
      text: "[1, 2, 3, 4, 5]"
      isCorrect: true
    - id: c
      text: "[[1, 2, 3], [4, 5]]"
      isCorrect: false
    - id: d
      text: "Error"
      isCorrect: false
  explanation: "The + operator concatenates lists, creating a new list with all elements from both lists."

- id: q5
  text: "Which of the following creates a list of squares from 0 to 9?"
  options:
    - id: a
      text: "[x^2 for x in range(10)]"
      isCorrect: false
    - id: b
      text: "[x**2 for x in range(10)]"
      isCorrect: true
    - id: c
      text: "list(x**2 for x in 10)"
      isCorrect: false
    - id: d
      text: "[squares(x) for x in range(10)]"
      isCorrect: false
  explanation: "Python uses ** for exponentiation, not ^. The correct list comprehension syntax is [x**2 for x in range(10)]."
