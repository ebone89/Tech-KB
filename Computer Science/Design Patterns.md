# Design Patterns

Design patterns are reusable solutions to common software design problems.

## 🏗️ Creational Patterns

### Singleton
Ensures a class has only one instance and provides global access to it.

```python
class Singleton:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

**Use Cases**: Database connections, loggers, configuration managers

### Factory Method
Creates objects without specifying the exact class to create.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class AnimalFactory:
    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        raise ValueError(f"Unknown animal type: {animal_type}")
```

### Builder
Constructs complex objects step by step.

### Prototype
Creates new objects by cloning existing ones.

### Abstract Factory
Creates families of related objects without specifying their concrete classes.

## 🔧 Structural Patterns

### Adapter
Allows incompatible interfaces to work together.

```python
class EuropeanSocket:
    def voltage(self):
        return 230

class USASocket:
    def voltage(self):
        return 120

class Adapter:
    def __init__(self, socket):
        self.socket = socket
    
    def voltage(self):
        return self.socket.voltage()
```

### Decorator
Adds new functionality to objects dynamically.

```python
def uppercase_decorator(func):
    def wrapper():
        result = func()
        return result.upper()
    return wrapper

@uppercase_decorator
def greet():
    return "hello"

print(greet())  # Output: HELLO
```

### Facade
Provides a simplified interface to a complex subsystem.

### Proxy
Provides a placeholder for another object to control access to it.

### Composite
Composes objects into tree structures to represent hierarchies.

### Bridge
Separates abstraction from implementation.

### Flyweight
Shares common state between multiple objects to save memory.

## 🎭 Behavioral Patterns

### Observer
Defines a one-to-many dependency between objects.

```python
class Subject:
    def __init__(self):
        self._observers = []
    
    def attach(self, observer):
        self._observers.append(observer)
    
    def notify(self, message):
        for observer in self._observers:
            observer.update(message)

class Observer:
    def update(self, message):
        print(f"Received: {message}")
```

**Use Cases**: Event handling, MVC pattern

### Strategy
Defines a family of algorithms and makes them interchangeable.

```python
class PaymentStrategy(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class CreditCardPayment(PaymentStrategy):
    def pay(self, amount):
        return f"Paying ${amount} with credit card"

class PayPalPayment(PaymentStrategy):
    def pay(self, amount):
        return f"Paying ${amount} with PayPal"
```

### Command
Encapsulates a request as an object.

### Iterator
Provides a way to access elements sequentially without exposing the underlying representation.

### Template Method
Defines the skeleton of an algorithm in a base class.

### Chain of Responsibility
Passes requests along a chain of handlers.

### State
Allows an object to alter its behavior when its internal state changes.

### Mediator
Defines an object that encapsulates how a set of objects interact.

### Memento
Captures and restores an object's internal state.

### Visitor
Separates algorithms from the objects on which they operate.

## 🎯 Pattern Selection Guide

**When you need to:**
- Create objects → Use Creational Patterns
- Organize class relationships → Use Structural Patterns
- Define communication between objects → Use Behavioral Patterns

## 📚 SOLID Principles

Design patterns often implement SOLID principles:

1. **S**ingle Responsibility Principle
2. **O**pen/Closed Principle
3. **L**iskov Substitution Principle
4. **I**nterface Segregation Principle
5. **D**ependency Inversion Principle

## Related Topics

- [[Object-Oriented Programming]]
- [[Software Development Life Cycle]]
- [[Code Review Best Practices]]

---

#designpatterns #softwareengineering #fundamentals
