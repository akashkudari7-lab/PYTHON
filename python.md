# Complete Python Guide: From Basics to Advanced with OOP

---

## Table of Contents
1. [Module 1: Python Fundamentals](#module-1-python-fundamentals)
2. [Module 2: Core Data Structures](#module-2-core-data-structures)
3. [Module 3: Functions & Functional Programming](#module-3-functions--functional-programming)
4. [Module 4: Object-Oriented Programming (OOP)](#module-4-object-oriented-programming-oop)
5. [Module 5: Advanced Python Features](#module-5-advanced-python-features)
6. [Module 6: Exception Handling & File I/O](#module-6-exception-handling--file-io)

---

# Module 1: Python Fundamentals

### 1. Variables & Dynamic Typing
Python is **dynamically typed** — you don't need to declare data types explicitly. Variables are references to objects in memory.

```python
x = 10          # int
price = 99.95   # float
name = "Antigravity" # str
is_active = True # bool
nothing = None   # NoneType

# Type checking & casting
print(type(x))         # <class 'int'>
str_val = str(x)       # "10"
float_val = float("3.14") # 3.14
```

### 2. Operators & Precedence
- **Arithmetic:** `+`, `-`, `*`, `/` (float div), `//` (floor div), `%` (modulo), `**` (exponentiation)
- **Comparison:** `==`, `!=`, `>`, `<`, `>=`, `<=`
- **Logical:** `and`, `or`, `not`
- **Identity & Membership:**
  - `is` vs `==`: `==` checks value equality; `is` checks reference/memory address equality.
  - `in` / `not in`: Tests membership in sequences/iterables.

```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)  # True (same values)
print(a is b)  # False (different objects in memory)
```

### 3. Control Flow & Match-Case

```python
# If-Elif-Else
score = 85
if score >= 90:
    grade = 'A'
elif score >= 80:
    grade = 'B'
else:
    grade = 'C'

# Ternary Operator (Conditional Expression)
status = "Adult" if age >= 18 else "Minor"

# Structural Pattern Matching (Python 3.10+)
command = "start"
match command:
    case "start":
        print("System starting...")
    case "stop" | "halt":
        print("System stopping...")
    case _:
        print("Unknown command")
```

### 4. Loops & `else` with Loops

```python
# for loop with range
for i in range(5):        # 0 to 4
    if i == 3:
        continue          # skip rest of iteration
    print(i)

# while loop
count = 3
while count > 0:
    count -= 1

# Loop-Else: The else block runs ONLY if the loop completes WITHOUT break
target = 7
numbers = [1, 3, 5, 9]
for n in numbers:
    if n == target:
        print("Found!")
        break
else:
    print("Target not found in list")  # This executes
```

---

# Module 2: Core Data Structures

### 1. Lists (`list`) — Mutable, Ordered Sequence
- Dynamic array under the hood. $O(1)$ random access, $O(1)$ amortized append.

```python
nums = [10, 20, 30, 40, 50]

# Slicing: [start:stop:step]
print(nums[1:4])     # [20, 30, 40]
print(nums[::-1])    # [50, 40, 30, 20, 10] (reversed)

# Common Methods
nums.append(60)      # Add to end: O(1)
nums.insert(1, 15)   # Insert at index: O(n)
nums.pop()           # Remove & return last: O(1)
nums.remove(20)      # Remove first occurrence: O(n)

# List Comprehensions
evens_squared = [x**2 for x in range(10) if x % 2 == 0]
```

### 2. Tuples (`tuple`) — Immutable, Ordered Sequence
- Hashable if all items inside are hashable (can be dictionary keys).
- Memory-efficient compared to lists.

```python
point = (10, 20)
x, y = point  # Tuple Unpacking

# Extended Unpacking
first, *middle, last = [1, 2, 3, 4, 5]
# first = 1, middle = [2, 3, 4], last = 5
```

### 3. Dictionaries (`dict`) — Mutable, Key-Value Pairs
- Implemented as hash tables: $O(1)$ average lookup, insert, and delete.

```python
user = {"name": "Alice", "role": "Engineer"}

# Safe retrieval with get()
age = user.get("age", 25)  # returns default 25 if key doesn't exist

# Iteration
for key, value in user.items():
    print(f"{key}: {value}")

# Dictionary Comprehension
square_map = {x: x**2 for x in range(5)} # {0:0, 1:1, 2:4, 3:9, 4:16}
```

### 4. Sets (`set`) — Mutable, Unordered, Unique Elements
- High-performance membership testing ($O(1)$).

```python
set_a = {1, 2, 3, 4}
set_b = {3, 4, 5, 6}

print(set_a | set_b)  # Union: {1, 2, 3, 4, 5, 6}
print(set_a & set_b)  # Intersection: {3, 4}
print(set_a - set_b)  # Difference: {1, 2}
print(set_a ^ set_b)  # Symmetric Difference: {1, 2, 5, 6}
```

---

# Module 3: Functions & Functional Programming

### 1. Function Arguments & Scope

```python
# *args (variable positional) & **kwargs (variable keyword arguments)
def custom_printer(title, *args, prefix="[LOG]", **kwargs):
    print(f"{prefix} {title}")
    for item in args:
        print(" -", item)
    for key, value in kwargs.items():
        print(f" {key} = {value}")

custom_printer("Report", 10, 20, level="INFO", author="Dev")
```

#### Variable Scope: LEGB Rule
- **L**ocal $\rightarrow$ **E**nclosing $\rightarrow$ **G**lobal $\rightarrow$ **B**uilt-in

```python
count = 0

def increment():
    global count
    count += 1

def outer():
    val = "outer"
    def inner():
        nonlocal val
        val = "modified"
    inner()
    return val  # returns "modified"
```

### 2. Lambda, Map, Filter, Reduce

```python
from functools import reduce

# Lambda (anonymous function)
add = lambda a, b: a + b

# Map
nums = [1, 2, 3, 4]
doubled = list(map(lambda x: x * 2, nums))  # [2, 4, 6, 8]

# Filter
odds = list(filter(lambda x: x % 2 != 0, nums))  # [1, 3]

# Reduce
product = reduce(lambda acc, x: acc * x, nums, 1)  # 24
```

### 3. Closures & Decorators
A **decorator** is a function that takes another function as an argument, extends its behavior, and returns a callable.

```python
import time
from functools import wraps

def timer_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        duration = time.perf_counter() - start
        print(f"Function '{func.__name__}' executed in {duration:.6f} seconds")
        return result
    return wrapper

@timer_decorator
def compute_heavy_task(n):
    return sum(i * i for i in range(n))

compute_heavy_task(100000)
```

---

# Module 4: Object-Oriented Programming (OOP)

OOP organizes code around **classes** (blueprints) and **objects** (instances).

### The 4 Pillars of OOP
1. **Encapsulation:** Bundling data and methods into a single unit and restricting direct access to internal details.
2. **Abstraction:** Hiding complex implementation details and showing only the essential interface.
3. **Inheritance:** Enabling a child class to acquire properties and methods of a parent class.
4. **Polymorphism:** Allowing different classes to be treated through the same interface.

---

### 1. Class & Object Anatomy

```python
class BankAccount:
    # Class Variable (shared across all instances)
    bank_name = "Global Trust Bank"
    interest_rate = 0.04

    def __init__(self, account_holder: str, initial_balance: float = 0.0):
        # Instance Variables (unique to each instance)
        self.holder = account_holder
        self._balance = initial_balance  # Protected convention

    # Instance Method: operates on instance data ('self')
    def deposit(self, amount: float) -> None:
        if amount > 0:
            self._balance += amount
            print(f"Deposited ${amount}. Balance: ${self._balance}")

    # Class Method: operates on class-level data ('cls')
    @classmethod
    def set_interest_rate(cls, new_rate: float) -> None:
        cls.interest_rate = new_rate

    # Static Method: independent utility function (no self or cls)
    @staticmethod
    def is_valid_account_number(acc_no: str) -> bool:
        return len(acc_no) == 10 and acc_no.isdigit()
```

---

### 2. Encapsulation & `@property` Decorator

In Python, access levels are indicated by naming conventions:
- `public_var`: Accessible anywhere.
- `_protected_var`: Internal use convention (accessible in subclasses).
- `__private_var`: Name-mangled to `_ClassName__private_var` to prevent accidental overriding.

```python
class Employee:
    def __init__(self, name: str, salary: float):
        self.name = name
        self.__salary = salary  # Private variable

    # Getter
    @property
    def salary(self) -> float:
        return self.__salary

    # Setter with validation
    @salary.setter
    def salary(self, value: float) -> None:
        if value < 0:
            raise ValueError("Salary cannot be negative")
        self.__salary = value

emp = Employee("Alice", 75000)
print(emp.salary)       # Accesses via getter: 75000
emp.salary = 85000      # Invokes setter
```

---

### 3. Inheritance & `super()`

```python
class Vehicle:
    def __init__(self, brand: str, model: str):
        self.brand = brand
        self.model = model

    def start_engine(self) -> str:
        return f"{self.brand} {self.model} engine started."

class ElectricCar(Vehicle):
    def __init__(self, brand: str, model: str, battery_capacity: int):
        # Call parent constructor
        super().__init__(brand, model)
        self.battery_capacity = battery_capacity

    # Method Overriding
    def start_engine(self) -> str:
        return f"{self.brand} {self.model} silently powered on with {self.battery_capacity}kWh battery."

car = ElectricCar("Tesla", "Model 3", 75)
print(car.start_engine())
```

#### Multiple Inheritance & Method Resolution Order (MRO)
Python uses the **C3 Linearization** algorithm to determine the lookup order for methods.

```python
class A:
    def action(self): return "A"

class B(A):
    def action(self): return "B"

class C(A):
    def action(self): return "C"

class D(B, C):
    pass

obj = D()
print(obj.action())      # Output: "B"
print(D.mro())           # [D, B, C, A, object]
```

---

### 4. Polymorphism & Duck Typing

> *"If it walks like a duck and quacks like a duck, it's a duck."*

```python
class PDFExporter:
    def export(self, data):
        return f"Exporting {data} to PDF"

class CSVExporter:
    def export(self, data):
        return f"Exporting {data} to CSV"

def generate_report(exporter, data):
    # Polymorphic call: doesn't care about the concrete type, only that it has export()
    print(exporter.export(data))

generate_report(PDFExporter(), "Financials")
generate_report(CSVExporter(), "Financials")
```

---

### 5. Abstraction & Abstract Base Classes (ABC)

Abstract classes cannot be instantiated and enforce interface contracts on child classes.

```python
from abc import ABC, abstractmethod

class PaymentGateway(ABC):
    @abstractmethod
    def authenticate(self) -> bool:
        pass

    @abstractmethod
    def process_payment(self, amount: float) -> bool:
        pass

class PayPalGateway(PaymentGateway):
    def authenticate(self) -> bool:
        print("Authenticating with PayPal API...")
        return True

    def process_payment(self, amount: float) -> bool:
        print(f"Processing ${amount} via PayPal")
        return True
```

---

### 6. Magic / Dunder (Double Underscore) Methods

Special methods that customize class behavior with built-in operations.

```python
class Vector:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    # String representation for developers
    def __repr__(self) -> str:
        return f"Vector({self.x}, {self.y})"

    # User-friendly string representation (print())
    def __str__(self) -> str:
        return f"({self.x}, {self.y})"

    # Operator Overloading for '+'
    def __add__(self, other: 'Vector') -> 'Vector':
        return Vector(self.x + other.x, self.y + other.y)

    # Equality check for '=='
    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Vector):
            return False
        return self.x == other.x and self.y == other.y

    # Length support for len()
    def __len__(self) -> int:
        return int((self.x**2 + self.y**2)**0.5)

v1 = Vector(3, 4)
v2 = Vector(1, 2)
print(v1 + v2)        # (4, 6)
print(len(v1))        # 5
```

---

# Module 5: Advanced Python Features

### 1. Iterators & Generators

- **Iterator:** Object implementing `__iter__()` and `__next__()`.
- **Generator:** Uses `yield` to stream values lazily one-by-one without loading everything into RAM.

```python
# Custom Generator Function
def fibonacci(limit: int):
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

for val in fibonacci(50):
    print(val, end=" ")
# 0 1 1 2 3 5 8 13 21 34
```

### 2. Context Managers (`with` Statement)
Ensures deterministic resource acquisition and release (e.g., closing file or database connections).

```python
# Class-based Context Manager
class DatabaseConnection:
    def __enter__(self):
        print("Connecting to database...")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing database connection.")
        return False  # Propagate exceptions if any

with DatabaseConnection():
    print("Executing query...")
```

### 3. Data Classes (`@dataclass`) (Python 3.7+)
Automatically generates `__init__`, `__repr__`, `__eq__` boilerplate.

```python
from dataclasses import dataclass, field

@dataclass(frozen=True)  # frozen=True makes instances immutable
class Product:
    name: str
    price: float
    tags: list[str] = field(default_factory=list)

p = Product("Laptop", 1200.0, ["electronics", "computers"])
print(p)  # Product(name='Laptop', price=1200.0, tags=['electronics', 'computers'])
```

---

# Module 6: Exception Handling & File I/O

### 1. Robust Exception Handling

```python
class InsufficientFundsError(Exception):
    """Custom application exception."""
    pass

def withdraw(balance: float, amount: float) -> float:
    if amount > balance:
        raise InsufficientFundsError(f"Cannot withdraw ${amount} from balance ${balance}")
    return balance - amount

try:
    new_bal = withdraw(100, 150)
except InsufficientFundsError as err:
    print(f"Transaction Failed: {err}")
except Exception as generic_err:
    print(f"Unexpected error: {generic_err}")
else:
    print(f"Transaction successful! New balance: ${new_bal}")
finally:
    print("Transaction session closed.")
```

### 2. File I/O Best Practices

```python
# Writing to a text file
with open("data.txt", "w", encoding="utf-8") as f:
    f.write("Line 1: Hello World\n")
    f.write("Line 2: Python Advanced Notes\n")

# Reading line-by-line efficiently
with open("data.txt", "r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())
```

---

## Complete Python Mastery Roadmap

```
1. Basics: Variables, Datatypes, Operators, Control Flow
   │
2. Data Structures: Lists, Tuples, Dictionaries, Sets
   │
3. Functions: Scope, *args/**kwargs, Lambdas, Closures, Decorators
   │
4. OOP: Classes, 4 Pillars, MRO, Dunder Methods, Abstraction
   │
5. Advanced: Iterators, Generators, Context Managers, Dataclasses
   │
6. Production: Unit Testing, Typing/Type Hints, Asyncio, Packaging
```
