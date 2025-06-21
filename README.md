### **Python Programming: Mastering Functions**

Functions are the fundamental building blocks of any significant Python program. They are the verbs of the language, allowing you to package functionality into reusable, organized, and readable blocks of code. Mastering functions means moving beyond simple definitions to understanding scope, advanced argument passing, and powerful paradigms like decorators and closures.

This guide will take you on that journey.

---

### **Table of Contents**

1.  **The Foundation: Defining and Calling**
2.  **Inputs & Outputs: Parameters, Arguments, and Return Values**
3.  **Flexible Signatures: Default, Keyword, and Arbitrary Arguments**
4.  **The LEGB Rule: Understanding Scope**
5.  **First-Class Citizens: Functions as Objects**
6.  **Power Tools: Lambdas, Map, and Filter**
7.  **The Pinnacle: Closures and Decorators**
8.  **Modern & Professional: Type Hinting and Docstrings**
9.  **Best Practices: The Zen of Functions**
10. **Quick Reference Cheat Sheet**

---

### **1. The Foundation: Defining and Calling**

At its core, a function is a named block of code that performs a specific task.

**Defining a Function:**
You use the `def` keyword, followed by the function name, parentheses `()`, and a colon `:`. The code block inside is indented.

```python
def greet():
    """This is a simple function that prints a greeting."""
    print("Hello, World!")
```

**Calling a Function:**
To execute the function, you "call" it by its name followed by parentheses.

```python
greet()  # Output: Hello, World!
```

---

### **2. Inputs & Outputs: Parameters, Arguments, and Return Values**

Functions become truly useful when they can process data.

*   **Parameter:** A variable listed inside the parentheses in the function definition.
*   **Argument:** The actual value sent to the function when you call it.

```python
def greet_person(name):  # 'name' is a parameter
    print(f"Hello, {name}!")

greet_person("Alice")  # "Alice" is an argument
```

**The `return` Statement:**
To send a value *back* from a function, use the `return` statement. A function without an explicit `return` statement implicitly returns `None`.

```python
def add(a, b):
    """Returns the sum of two numbers."""
    return a + b

result = add(5, 3)
print(result)  # Output: 8
```

---

### **3. Flexible Signatures: Default, Keyword, and Arbitrary Arguments**

**Default Arguments:**
You can provide a default value for a parameter, making it optional during the call.

```python
def power(base, exponent=2):
    return base ** exponent

print(power(3))      # Output: 9 (exponent defaults to 2)
print(power(3, 3))   # Output: 27 (exponent is overridden)
```

**Keyword vs. Positional Arguments:**
You can pass arguments by position or by name (keyword). Keyword arguments improve readability.

```python
# Positional
power(3, 4)

# Keyword (order doesn't matter)
power(exponent=4, base=3)
```

**Arbitrary Arguments: `*args` and `**kwargs`**
This is a cornerstone of flexible and advanced functions.

*   `*args`: Gathers any number of *positional* arguments into a **tuple**.
*   `**kwargs`: Gathers any number of *keyword* arguments into a **dictionary**.

```python
def process_data(file_name, *args, **kwargs):
    print(f"Processing file: {file_name}")

    if args:
        print(f"Positional data received: {args}")

    if kwargs:
        print(f"Keyword data received: {kwargs}")

# Example call
process_data(
    "data.csv",
    "param1",
    "param2",
    mode="fast",
    retries=3
)
# Output:
# Processing file: data.csv
# Positional data received: ('param1', 'param2')
# Keyword data received: {'mode': 'fast', 'retries': 3}
```

---

### **4. The LEGB Rule: Understanding Scope**

Scope determines the visibility of a variable. Python resolves names using the **LEGB** rule:

1.  **L**ocal: Variables defined inside the current function.
2.  **E**nclosing: Variables in the local scope of enclosing functions (for nested functions).
3.  **G**lobal: Variables defined at the top level of a module.
4.  **B**uilt-in: Names pre-assigned in Python (e.g., `print`, `len`).

```python
x = "global"  # Global scope

def outer_func():
    x = "enclosing"  # Enclosing scope
    def inner_func():
        x = "local"  # Local scope
        print(x)  # Python finds 'x' here first
    inner_func()

outer_func() # Output: local
```
*Use the `global` and `nonlocal` keywords to modify variables in outer scopes, but do so sparingly.*

---

### **5. First-Class Citizens: Functions as Objects**

In Python, functions are "first-class citizens." This means they can be treated like any other object (e.g., an integer or a string). You can:

1.  **Assign them to variables.**
2.  **Store them in data structures (lists, dictionaries).**
3.  **Pass them as arguments to other functions (Higher-Order Functions).**
4.  **Return them from other functions.**

```python
def shout(text):
    return text.upper()

# 1. Assign to a variable
yell = shout
print(yell("hello")) # Output: HELLO

# 3. Pass as an argument
def whisper(text):
    return text.lower()

def greet(func):
    # 'func' is a function passed as an argument
    greeting = func("Hi, I am a function passed as an argument.")
    print(greeting)

greet(shout)   # Output: HI, I AM A FUNCTION PASSED AS AN ARGUMENT.
greet(whisper) # Output: hi, i am a function passed as an argument.
```

---

### **6. Power Tools: Lambdas, Map, and Filter**

**Lambda Functions:**
A lambda is a small, anonymous, single-expression function. It's syntactic sugar for simple tasks.

**Syntax:** `lambda arguments: expression`

```python
# A normal function
def add(x, y):
  return x + y

# The equivalent lambda function
add_lambda = lambda x, y: x + y
print(add_lambda(2, 3)) # Output: 5
```
Lambdas are most useful when passed as arguments to higher-order functions like `sorted()`, `map()`, and `filter()`.

**`map()` and `filter()`:**
*   `map(function, iterable)`: Applies a function to every item of an iterable.
*   `filter(function, iterable)`: Creates an iterator from elements of an iterable for which the function returns `True`.

```python
numbers = [1, 2, 3, 4, 5]

# Use map with a lambda to square each number
squared_numbers = list(map(lambda x: x * x, numbers))
print(squared_numbers) # Output: [1, 4, 9, 16, 25]

# Use filter with a lambda to get only even numbers
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers) # Output: [2, 4]
```

---

### **7. The Pinnacle: Closures and Decorators**

**Closures:**
A closure occurs when a nested function "remembers" the enclosing scope's variables, even after the outer function has finished executing.

```python
def make_multiplier(n):
    # 'n' is a free variable from the enclosing scope
    def multiplier(x):
        # This inner function "remembers" n
        return x * n
    return multiplier # Returns the inner function

# Create functions that "remember" their 'n'
times3 = make_multiplier(3)
times5 = make_multiplier(5)

print(times3(10)) # Output: 30
print(times5(10)) # Output: 50
```

**Decorators:**
A decorator is a function that takes another function as an argument, adds some functionality (wraps it), and returns the new, enhanced function. It's syntactic sugar for closures and higher-order functions.

**Syntax:** Use the `@` symbol on the line before a function definition.

```python
# This is a decorator
def timer_decorator(func):
    import time
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs) # Call the original function
        end_time = time.time()
        print(f"'{func.__name__}' took {end_time - start_time:.4f} seconds to run.")
        return result
    return wrapper

@timer_decorator
def long_running_task(n):
    """A sample function that takes some time to run."""
    sum = 0
    for i in range(n):
        sum += i
    return sum

long_running_task(10000000)
# Output: 'long_running_task' took 0.3125 seconds to run.
```

---

### **8. Modern & Professional: Type Hinting and Docstrings**

**Type Hinting (PEP 484):**
Type hints add static type information to your code. They don't enforce types at runtime but are invaluable for code editors, static analysis tools (like MyPy), and readability.

**Syntax:** `param: type` and `-> return_type`

```python
def greet(name: str) -> str:
    return f"Hello, {name}"

from typing import List

def total(numbers: List[float]) -> float:
    return sum(numbers)
```

**Docstrings (PEP 257):**
A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. It becomes the `__doc__` attribute of that object.

**Use triple quotes `"""` and follow a standard format.**

```python
def calculate_area(length: float, width: float) -> float:
    """Calculates the area of a rectangle.

    Args:
        length (float): The length of the rectangle.
        width (float): The width of the rectangle.

    Returns:
        float: The calculated area of the rectangle.
    """
    if length <= 0 or width <= 0:
        raise ValueError("Length and width must be positive.")
    return length * width
```
You can then access this documentation using `help(calculate_area)` or `print(calculate_area.__doc__)`.

---

### **9. Best Practices: The Zen of Functions**

*   **Single Responsibility Principle (SRP):** A function should do one thing and do it well.
*   **Descriptive Naming:** Use `snake_case` for function names (e.g., `calculate_tax`). The name should clearly state what the function does.
*   **Keep them Small:** Shorter functions are easier to read, test, and debug.
*   **Don't Repeat Yourself (DRY):** If you find yourself writing the same code multiple times, it's time to create a function.
*   **Prefer Pure Functions:** A pure function's return value is determined only by its input values, and it has no side effects (like modifying a global variable or printing to the console). They are easier to reason about and test.

---

### **10. Quick Reference Cheat Sheet**

| Concept | Syntax Example | Description |
| :--- | :--- | :--- |
| **Basic Definition** | `def my_func(): ...` | Defines a new function. |
| **Parameters** | `def my_func(a, b): ...` | Variables that receive input values. |
| **Return Value** | `return a + b` | Sends a result back from the function. |
| **Default Argument** | `def my_func(a=1): ...` | Makes an argument optional. |
| **Keyword Argument**| `my_func(a=5)` | Passes arguments by name for clarity. |
| **`*args`** | `def my_func(*args): ...` | Collects positional arguments into a tuple. |
| **`**kwargs`** | `def my_func(**kwargs): ...`| Collects keyword arguments into a dictionary. |
| **Lambda** | `lambda x: x * 2` | A small, anonymous, single-line function. |
| **Decorator** | `@my_decorator` | Modifies or enhances a function. |
| **Type Hint** | `def f(a: int) -> str:` | Documents expected types for clarity & tools. |
| **Docstring** | `"""Does a thing."""` | Documents the function's purpose for `help()`.|
