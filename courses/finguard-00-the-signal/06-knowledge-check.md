---
id: "finguard_00_06"
title: "Knowledge Check"
type: "quiz"
xp: 50
---

# Knowledge Check

Let's verify your understanding of the fundamentals before moving forward.

<!-- SEPARATOR -->

# questions
- id: q1
  text: "What do computers fundamentally understand?"
  options:
    - id: a
      text: "Numbers and text"
      isCorrect: false
    - id: b
      text: "Python code directly"
      isCorrect: false
    - id: c
      text: "Binary (patterns of 1s and 0s)"
      isCorrect: true
    - id: d
      text: "English instructions"
      isCorrect: false
  explanation: "Computers only understand two states: high voltage (1) and low voltage (0). Everything else — numbers, text, images — must be translated into binary patterns."

- id: q2
  text: "What is a variable in Python?"
  options:
    - id: a
      text: "A mathematical equation"
      isCorrect: false
    - id: b
      text: "A labeled container that stores data in memory"
      isCorrect: true
    - id: c
      text: "A type of function"
      isCorrect: false
    - id: d
      text: "A file on your computer"
      isCorrect: false
  explanation: "Variables are your program's memory. Each variable is a labeled container that holds one piece of data while the program runs."

- id: q3
  text: "In the assignment `transaction_amount = 1500`, which side holds the label (variable name)?"
  options:
    - id: a
      text: "The left side"
      isCorrect: true
    - id: b
      text: "The right side"
      isCorrect: false
    - id: c
      text: "Both sides"
      isCorrect: false
    - id: d
      text: "Neither side"
      isCorrect: false
  explanation: "In Python assignment, the label (variable name) goes on the left, the value goes on the right. The `=` operator stores the right value into the left label."

- id: q4
  text: "Why was Python chosen for FinGuard and data engineering in general?"
  options:
    - id: a
      text: "It's the fastest programming language"
      isCorrect: false
    - id: b
      text: "It was invented for banking specifically"
      isCorrect: false
    - id: c
      text: "Readable syntax, rich ecosystem, and large community"
      isCorrect: true
    - id: d
      text: "It's the only language that handles numbers"
      isCorrect: false
  explanation: "Python dominates data engineering because its syntax is readable (almost like English), it has powerful libraries (Pandas, NumPy), and millions of developers contribute solutions."

- id: q5
  text: "What is the purpose of the `bin()` function?"
  options:
    - id: a
      text: "To create a new variable"
      isCorrect: false
    - id: b
      text: "To delete data"
      isCorrect: false
    - id: c
      text: "To show the binary representation of a number"
      isCorrect: true
    - id: d
      text: "To convert text to numbers"
      isCorrect: false
  explanation: "The `bin()` function reveals how Python stores a number internally as binary. For example, `bin(1500)` returns `'0b10111011100'`."

- id: q6
  text: "What does 'programming' mean?"
  options:
    - id: a
      text: "Installing software on a computer"
      isCorrect: false
    - id: b
      text: "Using a computer to browse the internet"
      isCorrect: false
    - id: c
      text: "Writing step-by-step instructions for a computer to follow"
      isCorrect: true
    - id: d
      text: "Designing the physical hardware of a computer"
      isCorrect: false
  explanation: "Programming is writing instructions that tell the computer exactly what to do, step by step. The computer follows these instructions in order."

- id: q7
  text: "Why do programming languages exist?"
  options:
    - id: a
      text: "To make computers slower"
      isCorrect: false
    - id: b
      text: "To hide information from users"
      isCorrect: false
    - id: c
      text: "To provide human-readable instructions that translate to binary"
      isCorrect: true
    - id: d
      text: "To replace mathematics"
      isCorrect: false
  explanation: "Writing binary directly (`10110000 01100001`) is painful and error-prone. Programming languages let us write human-readable code that gets automatically translated to binary."

- id: q8
  text: "In the FinGuard project, what is the main goal you're building toward?"
  options:
    - id: a
      text: "A video game"
      isCorrect: false
    - id: b
      text: "A social media platform"
      isCorrect: false
    - id: c
      text: "A fraud detection and regulatory reporting system"
      isCorrect: true
    - id: d
      text: "A weather forecasting app"
      isCorrect: false
  explanation: "FinGuard is a Real-Time Fraud Detection & Regulatory Reporting System for banking. You'll detect suspicious transactions and generate compliance reports."
