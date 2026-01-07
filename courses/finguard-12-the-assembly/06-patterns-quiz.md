---
id: "finguard_12_06"
title: "Design Patterns Quiz"
type: "quiz"
xp: 50
---

# Design Patterns Quiz

Can you identify when to use each pattern?

<!-- SEPARATOR -->

# questions
- id: q1
  text: "When should you use the Factory pattern?"
  options:
    - id: a
      text: "When you need to store data"
      isCorrect: false
    - id: b
      text: "When you want to observe events"
      isCorrect: false
    - id: c
      text: "When object creation logic is complex or you want to hide implementation details"
      isCorrect: true
    - id: d
      text: "When building objects with many optional parameters"
      isCorrect: false
  explanation: "Factory encapsulates creation logic. The client says 'give me a Transaction' without knowing which concrete class gets instantiated. Useful when creation logic varies by type."

- id: q2
  text: "What does a Factory return?"
  options:
    - id: a
      text: "Always the same object"
      isCorrect: false
    - id: b
      text: "A string describing the object"
      isCorrect: false
    - id: c
      text: "An instance of a class, often determined by input parameters"
      isCorrect: true
    - id: d
      text: "Nothing — it only validates"
      isCorrect: false
  explanation: "A Factory creates and returns objects. Based on input (like 'checking' vs 'savings'), it decides which concrete class to instantiate and returns it."

- id: q3
  text: "What problem does the Repository pattern solve?"
  options:
    - id: a
      text: "Object creation complexity"
      isCorrect: false
    - id: b
      text: "Separating data access from business logic"
      isCorrect: true
    - id: c
      text: "Notifying multiple observers"
      isCorrect: false
    - id: d
      text: "Building complex objects step by step"
      isCorrect: false
  explanation: "Repository abstracts data storage. Your business logic calls `repository.save(txn)` without knowing if it's SQLite, PostgreSQL, or an in-memory store. Easy to swap and test."

- id: q4
  text: "Which is a benefit of the Repository pattern?"
  options:
    - id: a
      text: "Makes database queries faster"
      isCorrect: false
    - id: b
      text: "Eliminates the need for a database"
      isCorrect: false
    - id: c
      text: "Allows swapping storage implementations without changing business logic"
      isCorrect: true
    - id: d
      text: "Automatically creates backups"
      isCorrect: false
  explanation: "With Repository, you can use `InMemoryRepository` for tests and `PostgresRepository` for production. Your business logic code stays exactly the same."

- id: q5
  text: "When should you use the Strategy pattern?"
  options:
    - id: a
      text: "When you need to create objects"
      isCorrect: false
    - id: b
      text: "When you have multiple algorithms that should be interchangeable at runtime"
      isCorrect: true
    - id: c
      text: "When you need to notify multiple listeners"
      isCorrect: false
    - id: d
      text: "When storing data"
      isCorrect: false
  explanation: "Strategy encapsulates algorithms. If fee calculation varies by customer tier (Standard, Premium, VIP), each is a strategy. Swap them at runtime without changing the calculator."

- id: q6
  text: "What eliminates long `if/elif/elif` chains based on type?"
  options:
    - id: a
      text: "Factory pattern"
      isCorrect: false
    - id: b
      text: "Repository pattern"
      isCorrect: false
    - id: c
      text: "Strategy pattern"
      isCorrect: true
    - id: d
      text: "Builder pattern"
      isCorrect: false
  explanation: "Instead of `if type == 'standard': ... elif type == 'premium': ...`, you inject the appropriate Strategy object. Each strategy handles its own logic — no conditionals needed."

- id: q7
  text: "What is the Observer pattern used for?"
  options:
    - id: a
      text: "Creating objects"
      isCorrect: false
    - id: b
      text: "Storing data"
      isCorrect: false
    - id: c
      text: "Building complex objects"
      isCorrect: false
    - id: d
      text: "Notifying multiple objects when an event occurs"
      isCorrect: true
  explanation: "Observer establishes a subscription mechanism. When a transaction is processed, all observers (AuditLogger, EmailNotifier, FraudDetector) are notified automatically."

- id: q8
  text: "In the Observer pattern, what is the 'Subject'?"
  options:
    - id: a
      text: "The object that receives notifications"
      isCorrect: false
    - id: b
      text: "The object that maintains a list of observers and notifies them"
      isCorrect: true
    - id: c
      text: "The message being sent"
      isCorrect: false
    - id: d
      text: "The interface all observers implement"
      isCorrect: false
  explanation: "The Subject (e.g., `TransactionProcessor`) maintains the observer list and calls each observer's notification method when events occur."

- id: q9
  text: "When should you use the Builder pattern?"
  options:
    - id: a
      text: "When you need one object to notify many others"
      isCorrect: false
    - id: b
      text: "When algorithms need to be swapped at runtime"
      isCorrect: false
    - id: c
      text: "When you want to hide which class is instantiated"
      isCorrect: false
    - id: d
      text: "When constructing objects with many optional parameters"
      isCorrect: true
  explanation: "Builder avoids constructors with 15 parameters. Instead: `.title('Report').dateRange(start, end).includeDeposits().asPDF().build()` — readable and flexible."

- id: q10
  text: "What does a Builder's `build()` method do?"
  options:
    - id: a
      text: "Starts the building process"
      isCorrect: false
    - id: b
      text: "Adds another parameter"
      isCorrect: false
    - id: c
      text: "Creates and returns the final constructed object"
      isCorrect: true
    - id: d
      text: "Resets the builder"
      isCorrect: false
  explanation: "After chaining configuration methods (`.title()`, `.dateRange()`, etc.), `build()` validates settings and constructs the final immutable object."

- id: q11
  text: "Problem: 'I need to create different Account types based on a string input.' Which pattern?"
  options:
    - id: a
      text: "Factory"
      isCorrect: true
    - id: b
      text: "Repository"
      isCorrect: false
    - id: c
      text: "Strategy"
      isCorrect: false
    - id: d
      text: "Observer"
      isCorrect: false
  explanation: "Factory decides which class to instantiate based on input. `AccountFactory.create('checking')` returns a `CheckingAccount` — the client doesn't know the concrete class."

- id: q12
  text: "Problem: 'When fraud is detected, I need to log it, email the team, AND update the dashboard.' Which pattern?"
  options:
    - id: a
      text: "Factory"
      isCorrect: false
    - id: b
      text: "Repository"
      isCorrect: false
    - id: c
      text: "Strategy"
      isCorrect: false
    - id: d
      text: "Observer"
      isCorrect: true
  explanation: "Observer handles one-to-many notifications. The FraudDetector (subject) notifies all observers (Logger, EmailNotifier, Dashboard) without knowing their details."

- id: q13
  text: "Problem: 'Fee calculation is different for Standard, Premium, and VIP customers.' Which pattern?"
  options:
    - id: a
      text: "Factory"
      isCorrect: false
    - id: b
      text: "Repository"
      isCorrect: false
    - id: c
      text: "Strategy"
      isCorrect: true
    - id: d
      text: "Observer"
      isCorrect: false
  explanation: "Strategy encapsulates interchangeable algorithms. Each tier (Standard, Premium, VIP) is a strategy with its own `calculate_fee()` implementation."

- id: q14
  text: "Problem: 'I want to test my fraud detector without a real database.' Which pattern?"
  options:
    - id: a
      text: "Factory"
      isCorrect: false
    - id: b
      text: "Repository"
      isCorrect: true
    - id: c
      text: "Strategy"
      isCorrect: false
    - id: d
      text: "Builder"
      isCorrect: false
  explanation: "Repository abstracts storage. Inject `InMemoryRepository` for tests instead of `PostgresRepository`. Same interface, different implementation — perfect for testing."
