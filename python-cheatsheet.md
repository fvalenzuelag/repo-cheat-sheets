# Python Cheat Sheet

## Basics

### Variables and Data Types
```python
# Variable assignment
x = 5                # Integer
y = 2.5              # Float
name = "Python"      # String
is_active = True     # Boolean
my_list = [1, 2, 3]  # List
my_tuple = (1, 2, 3) # Tuple
my_dict = {"key": "value"}  # Dictionary
my_set = {1, 2, 3}   # Set
nothing = None       # None type
```

### String Operations
```python
s = "Hello World"
print(len(s))        # 11 (length)
print(s.upper())     # HELLO WORLD
print(s.lower())     # hello world
print(s.strip())     # Remove whitespace
print(s.split(" "))  # ['Hello', 'World']
print(s.replace("H", "J"))  # Jello World
print("Hello" in s)  # True
print(s[0:5])        # Hello (slicing)
print(f"{s}!")       # Hello World! (f-string)
```

### Lists
```python
fruits = ["apple", "banana", "cherry"]
print(len(fruits))   # 3
print(fruits[0])     # apple
fruits.append("orange")
fruits.insert(1, "mango")
fruits.remove("banana")
popped = fruits.pop()
fruits.sort()
fruits.reverse()
fruits_copy = fruits.copy()
```

### Dictionaries
```python
user = {
    "name": "John",
    "age": 30,
    "is_admin": False
}
print(user["name"])  # John
user["email"] = "john@example.com"  # Add new key-value
user.get("phone", "Not found")  # Get with default value
user.pop("age")  # Remove key-value
print(user.keys())  # Get all keys
print(user.values())  # Get all values
print(user.items())  # Get all key-value pairs
```

### Control Flow
```python
# If-Else
if x > 5:
    print("x is greater than 5")
elif x == 5:
    print("x is 5")
else:
    print("x is less than 5")

# For loops
for item in my_list:
    print(item)

for i in range(5):  # 0 to 4
    print(i)

# While loops
count = 0
while count < 5:
    print(count)
    count += 1
```

## Functions

### Basic Functions
```python
def greet(name):
    return f"Hello, {name}!"

result = greet("Python")  # "Hello, Python!"

# Default parameters
def greet(name="World"):
    return f"Hello, {name}!"

# Variable number of arguments
def sum_all(*args):
    return sum(args)

total = sum_all(1, 2, 3, 4)  # 10

# Keyword arguments
def build_profile(**kwargs):
    return kwargs

profile = build_profile(name="John", age=30)  # {'name': 'John', 'age': 30}
```

### Lambda Functions
```python
# Anonymous function
square = lambda x: x * x
print(square(5))  # 25

# With map()
numbers = [1, 2, 3, 4]
squared = list(map(lambda x: x * x, numbers))  # [1, 4, 9, 16]

# With filter()
evens = list(filter(lambda x: x % 2 == 0, numbers))  # [2, 4]
```

## Classes and Objects

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def greet(self):
        return f"Hello, my name is {self.name}!"
        
    @classmethod
    def from_birth_year(cls, name, birth_year):
        return cls(name, 2023 - birth_year)
        
    @staticmethod
    def is_adult(age):
        return age >= 18

# Creating objects
person1 = Person("Alice", 30)
print(person1.greet())  # "Hello, my name is Alice!"

# Inheritance
class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id
```

## Working with Files

```python
# Reading files
with open("file.txt", "r") as file:
    content = file.read()  # Read entire file
    
with open("file.txt", "r") as file:
    lines = file.readlines()  # Read lines into list
    
with open("file.txt", "r") as file:
    for line in file:  # Read line by line
        print(line.strip())
        
# Writing files
with open("file.txt", "w") as file:
    file.write("Hello World")
    
with open("file.txt", "a") as file:
    file.write("\nAppended text")
```

## Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
except Exception as e:
    print(f"Error occurred: {e}")
else:
    print("No errors occurred")
finally:
    print("This will always execute")
    
# Raising exceptions
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero!")
    return a / b
```

## Modules and Packages

```python
# Importing modules
import math
print(math.sqrt(16))  # 4.0

# Import specific functions
from math import sqrt, pi
print(sqrt(16))  # 4.0

# Import with alias
import numpy as np
arr = np.array([1, 2, 3])

# Create your own module (in my_module.py)
def my_function():
    return "Hello from my module!"

# Then import it
import my_module
my_module.my_function()
```

## List Comprehensions

```python
# Basic list comprehension
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With conditional
even_squares = [x**2 for x in range(10) if x % 2 == 0]  # [0, 4, 16, 36, 64]

# Dictionary comprehension
square_dict = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Set comprehension
unique_lengths = {len(word) for word in ["hello", "world", "python"]}  # {5, 6}
```

## Generators

```python
# Generator function
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# Using a generator
for i in countdown(5):
    print(i)  # 5, 4, 3, 2, 1

# Generator expression
squares_gen = (x**2 for x in range(10))
print(next(squares_gen))  # 0
```

## Decorators

```python
# Basic decorator
def my_decorator(func):
    def wrapper():
        print("Something before")
        func()
        print("Something after")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

# With arguments
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello {name}")
```

## Common Libraries

### Working with Data
```python
# NumPy for numerical operations
import numpy as np
arr = np.array([1, 2, 3])
arr = np.zeros((3, 3))
arr = np.arange(0, 10, 2)

# Pandas for data analysis
import pandas as pd
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
df.head()
df.describe()
df['A'].mean()
```

### Web Requests
```python
import requests
response = requests.get("https://api.example.com/data")
data = response.json()
```

### Date and Time
```python
from datetime import datetime, timedelta
now = datetime.now()
yesterday = now - timedelta(days=1)
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
```

### Regular Expressions
```python
import re
pattern = r"\d+"
matches = re.findall(pattern, "There are 3 apples and 5 oranges")  # ['3', '5']
```

### Virtual Environments
```bash
# Creating virtual environments
python -m venv myenv

# Activating (Windows)
myenv\Scripts\activate

# Activating (macOS/Linux)
source myenv/bin/activate

# Installing packages
pip install package_name

# Requirements file
pip freeze > requirements.txt
pip install -r requirements.txt
```
