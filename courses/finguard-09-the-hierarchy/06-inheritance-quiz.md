---
id: "finguard_09_06"
title: "Inheritance & Polymorphism Quiz"
type: "quiz"
xp: 50
---

# Inheritance & Polymorphism Quiz

Test your understanding of object-oriented design concepts.

<!-- SEPARATOR -->

# questions
- id: q1
  text: "What does inheritance allow you to do?"
  options:
    - id: a
      text: "Delete parent class methods"
      isCorrect: false
    - id: b
      text: "Create new classes that reuse and extend existing class behavior"
      isCorrect: true
    - id: c
      text: "Make classes run faster"
      isCorrect: false
    - id: d
      text: "Prevent code from being modified"
      isCorrect: false
  explanation: "Inheritance lets you create child classes that inherit attributes and methods from a parent class, then add or modify behavior as needed."

- id: q2
  text: "What does `super().__init__()` do in a child class constructor?"
  options:
    - id: a
      text: "Deletes the parent class"
      isCorrect: false
    - id: b
      text: "Creates a new instance of the child class"
      isCorrect: false
    - id: c
      text: "Calls the parent class's constructor"
      isCorrect: true
    - id: d
      text: "Prevents the child from having its own attributes"
      isCorrect: false
  explanation: "`super().__init__()` calls the parent class's `__init__` method, ensuring the parent's initialization logic runs before the child adds its own attributes."

- id: q3
  text: "When should you use inheritance (the 'is-a' relationship)?"
  options:
    - id: a
      text: "When two classes share a database"
      isCorrect: false
    - id: b
      text: "When a child class is a specialized type of the parent class"
      isCorrect: true
    - id: c
      text: "When you want to hide code"
      isCorrect: false
    - id: d
      text: "When classes need to communicate"
      isCorrect: false
  explanation: "Use inheritance when there's a true 'is-a' relationship. A `CheckingAccount` IS A `BankAccount`. A `Dog` IS AN `Animal`."

- id: q4
  text: "What is method overriding?"
  options:
    - id: a
      text: "Deleting a method from the parent class"
      isCorrect: false
    - id: b
      text: "Calling a method multiple times"
      isCorrect: false
    - id: c
      text: "Providing a new implementation of a parent method in a child class"
      isCorrect: true
    - id: d
      text: "Making a method private"
      isCorrect: false
  explanation: "Method overriding means a child class provides its own implementation of a method that exists in the parent class. The child's version is used instead."

- id: q5
  text: "What is polymorphism?"
  options:
    - id: a
      text: "Having multiple constructors"
      isCorrect: false
    - id: b
      text: "A class with no methods"
      isCorrect: false
    - id: c
      text: "The ability to treat different types uniformly through a common interface"
      isCorrect: true
    - id: d
      text: "Converting between data types"
      isCorrect: false
  explanation: "Polymorphism means 'many forms.' You can call the same method on different objects (e.g., `account.withdraw()`), and each object responds according to its own implementation."

- id: q6
  text: "What does `@abstractmethod` indicate?"
  options:
    - id: a
      text: "The method runs automatically"
      isCorrect: false
    - id: b
      text: "The method cannot be called"
      isCorrect: false
    - id: c
      text: "Subclasses must implement this method"
      isCorrect: true
    - id: d
      text: "The method is private"
      isCorrect: false
  explanation: "`@abstractmethod` marks a method that has no implementation in the base class. Any subclass MUST provide its own implementation, or it cannot be instantiated."

- id: q7
  text: "Which scenario violates the Liskov Substitution Principle?"
  options:
    - id: a
      text: "A `SavingsAccount` that adds an interest rate attribute"
      isCorrect: false
    - id: b
      text: "A `FrozenAccount` whose `withdraw()` method always returns False"
      isCorrect: true
    - id: c
      text: "A `CheckingAccount` that allows overdraft"
      isCorrect: false
    - id: d
      text: "A `BusinessAccount` with higher transaction limits"
      isCorrect: false
  explanation: "LSP says subtypes must be substitutable for their base types. A `FrozenAccount` that never allows withdrawal breaks expectations — code expecting an `Account` would fail unexpectedly."

- id: q8
  text: "What is the benefit of using abstract base classes (ABC)?"
  options:
    - id: a
      text: "They make code run faster"
      isCorrect: false
    - id: b
      text: "They reduce memory usage"
      isCorrect: false
    - id: c
      text: "They enforce a contract that subclasses must follow"
      isCorrect: true
    - id: d
      text: "They allow multiple inheritance"
      isCorrect: false
  explanation: "ABCs define a contract. If your class inherits from an ABC but doesn't implement all abstract methods, Python raises an error at instantiation — catching mistakes early."

- id: q9
  text: "Given `class Account: ...`, `class CheckingAccount(Account): ...`, `class SavingsAccount(Account): ...`, which statement is TRUE?"
  options:
    - id: a
      text: "`CheckingAccount` and `SavingsAccount` inherit from each other"
      isCorrect: false
    - id: b
      text: "Both `CheckingAccount` and `SavingsAccount` can be used wherever `Account` is expected"
      isCorrect: true
    - id: c
      text: "`Account` inherits from `CheckingAccount`"
      isCorrect: false
    - id: d
      text: "These classes cannot share any methods"
      isCorrect: false
  explanation: "Both child classes inherit from `Account`, so they can be used polymorphically wherever an `Account` is expected. This is the power of inheritance + polymorphism."

- id: q10
  text: "When should you prefer composition over inheritance?"
  options:
    - id: a
      text: "Never — inheritance is always better"
      isCorrect: false
    - id: b
      text: "When you want code reuse"
      isCorrect: false
    - id: c
      text: "When objects have a 'has-a' relationship rather than 'is-a'"
      isCorrect: true
    - id: d
      text: "When working with abstract classes"
      isCorrect: false
  explanation: "Use composition when the relationship is 'has-a.' An `Account` HAS A `FeeCalculator`, so you inject the calculator rather than inheriting from it. This provides more flexibility."
