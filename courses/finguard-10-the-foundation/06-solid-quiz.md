---
id: "finguard_10_06"
title: "SOLID Principles Quiz"
type: "quiz"
xp: 50
---

# SOLID Principles Quiz

Can you identify which principles are being followed or violated?

<!-- SEPARATOR -->

# questions
- id: q1
  text: "What does the 'S' in SOLID stand for?"
  options:
    - id: a
      text: "Secure Responsibility Principle"
      isCorrect: false
    - id: b
      text: "Single Responsibility Principle"
      isCorrect: true
    - id: c
      text: "Shared Responsibility Principle"
      isCorrect: false
    - id: d
      text: "Static Responsibility Principle"
      isCorrect: false
  explanation: "The Single Responsibility Principle states that a class should have only one reason to change — it should do one thing well."

- id: q2
  text: "Which class name suggests a likely SRP violation?"
  options:
    - id: a
      text: "`TransactionValidator`"
      isCorrect: false
    - id: b
      text: "`EmailNotifier`"
      isCorrect: false
    - id: c
      text: "`TransactionManagerAndReporter`"
      isCorrect: true
    - id: d
      text: "`FraudDetector`"
      isCorrect: false
  explanation: "Class names with 'And' or 'Manager' often signal multiple responsibilities. `TransactionManagerAndReporter` clearly does two things that should be separate."

- id: q3
  text: "What does the Open/Closed Principle state?"
  options:
    - id: a
      text: "Classes should be open for modification"
      isCorrect: false
    - id: b
      text: "Classes should be closed to extension"
      isCorrect: false
    - id: c
      text: "Classes should be open for extension but closed for modification"
      isCorrect: true
    - id: d
      text: "Classes should never change"
      isCorrect: false
  explanation: "OCP means you should be able to add new behavior (extension) without changing existing code (modification). This is typically achieved through polymorphism."

- id: q4
  text: "Which code pattern often violates OCP?"
  options:
    - id: a
      text: "Using abstract base classes"
      isCorrect: false
    - id: b
      text: "Growing `if/elif/elif` chains based on type"
      isCorrect: true
    - id: c
      text: "Using dependency injection"
      isCorrect: false
    - id: d
      text: "Creating interfaces"
      isCorrect: false
  explanation: "Long `if/elif` chains that check types require modification every time you add a new type. Polymorphism eliminates this by letting each type define its own behavior."

- id: q5
  text: "What does the Liskov Substitution Principle require?"
  options:
    - id: a
      text: "All classes must have a parent"
      isCorrect: false
    - id: b
      text: "Subclasses must be smaller than parents"
      isCorrect: false
    - id: c
      text: "Subclasses must be usable wherever the parent class is expected"
      isCorrect: true
    - id: d
      text: "Parent classes cannot have methods"
      isCorrect: false
  explanation: "LSP says if `B` extends `A`, you should be able to use `B` anywhere `A` is expected without breaking behavior. Subclasses must honor the parent's contract."

- id: q6
  text: "A `Bird` class has a `fly()` method. You create a `Penguin` subclass. What's the LSP-compliant solution?"
  options:
    - id: a
      text: "Make `Penguin.fly()` raise an exception"
      isCorrect: false
    - id: b
      text: "Make `Penguin.fly()` do nothing"
      isCorrect: false
    - id: c
      text: "Refactor: create `FlyingBird` and `NonFlyingBird` base classes"
      isCorrect: true
    - id: d
      text: "Delete the `Penguin` class"
      isCorrect: false
  explanation: "If `Penguin` can't fly, it shouldn't inherit a `fly()` promise. The solution is better abstraction — separate `FlyingBird` from `Bird` so penguins don't inherit impossible behavior."

- id: q7
  text: "What does the Interface Segregation Principle state?"
  options:
    - id: a
      text: "Interfaces should be as large as possible"
      isCorrect: false
    - id: b
      text: "Clients should not be forced to depend on methods they don't use"
      isCorrect: true
    - id: c
      text: "Every class needs an interface"
      isCorrect: false
    - id: d
      text: "Interfaces cannot have multiple methods"
      isCorrect: false
  explanation: "ISP says don't create 'fat' interfaces. If a class only needs 2 of 10 methods, split the interface. Classes shouldn't implement methods they don't need."

- id: q8
  text: "A `ReportGenerator` ABC requires `generate_pdf()`, `generate_csv()`, `generate_excel()`, `send_email()`, and `upload_to_cloud()`. Which principle does it violate?"
  options:
    - id: a
      text: "Single Responsibility"
      isCorrect: false
    - id: b
      text: "Open/Closed"
      isCorrect: false
    - id: c
      text: "Interface Segregation"
      isCorrect: true
    - id: d
      text: "Dependency Inversion"
      isCorrect: false
  explanation: "This is a 'fat interface.' A class that only needs CSV generation is forced to implement PDF, Excel, email, and cloud upload. Split into smaller, focused interfaces."

- id: q9
  text: "What does the Dependency Inversion Principle state?"
  options:
    - id: a
      text: "Low-level modules should depend on high-level modules"
      isCorrect: false
    - id: b
      text: "Dependencies should be avoided entirely"
      isCorrect: false
    - id: c
      text: "High-level modules should depend on abstractions, not concrete implementations"
      isCorrect: true
    - id: d
      text: "All dependencies must be injected"
      isCorrect: false
  explanation: "DIP says depend on abstractions (interfaces), not concrete classes. Your `FraudDetector` should depend on `AlertRepository` (abstract), not `PostgresAlertRepository` (concrete)."

- id: q10
  text: "Option A: `self.db = PostgresDatabase()` in `__init__`. Option B: `def __init__(self, repository: AlertRepository)`. Which follows DIP?"
  options:
    - id: a
      text: "Option A"
      isCorrect: false
    - id: b
      text: "Option B"
      isCorrect: true
    - id: c
      text: "Both"
      isCorrect: false
    - id: d
      text: "Neither"
      isCorrect: false
  explanation: "Option B injects an abstraction (`AlertRepository`). Option A creates a concrete dependency internally. B is testable (inject mock), flexible (swap implementations), and follows DIP."

- id: q11
  text: "You need to add a new transaction type. With good OCP design, what do you do?"
  options:
    - id: a
      text: "Modify the existing `TransactionProcessor` class"
      isCorrect: false
    - id: b
      text: "Add another `elif` branch"
      isCorrect: false
    - id: c
      text: "Create a new class implementing the `Transaction` interface"
      isCorrect: true
    - id: d
      text: "Delete the old transaction types"
      isCorrect: false
  explanation: "With OCP, you extend by creating new classes that implement existing interfaces. The core processor doesn't change — you just register the new type."

- id: q12
  text: "Which SOLID principle does dependency injection primarily support?"
  options:
    - id: a
      text: "Single Responsibility"
      isCorrect: false
    - id: b
      text: "Open/Closed"
      isCorrect: false
    - id: c
      text: "Liskov Substitution"
      isCorrect: false
    - id: d
      text: "Dependency Inversion"
      isCorrect: true
  explanation: "Dependency injection is the mechanism for applying DIP. Instead of creating dependencies internally, you inject abstractions from outside — making code flexible and testable."
