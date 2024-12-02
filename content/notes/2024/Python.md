---
title: Python
date: 2024-05-09T12:16:34.3434+05:30
draft: false
tags:
  - programming
---

## Data types

```
- Numeric Types
  - int
  - float
  - complex
- Sequence Types
  - str #""
  - list #[]
  - tuple #()
- Mapping Type
  - dict #{}
- Set Types
  - set #{}
  - frozenset
- Boolean Type
  - bool #True/False
- Binary Types
  - bytes #[]
  - bytearray
  - memoryview
```
 
## Mutable and Imutable

Mutable data types are those whose contents can be changed after creation, while immutable data types cannot be changed once they are created.

Mutable data types:
1. Lists
2. Dictionaries
3. Sets
4. Byte arrays

Immutable data types:
1. Integers (`int`)
2. Floating-point numbers (`float`)
3. Complex numbers (`complex`)
4. Strings (`str`)
5. Tuples (`tuple`)
6. Frozensets (`frozenset`)
7. Bytes (`bytes`)
8. Booleans (`bool`)

All mutable data type will point to same memory when they have same vaule 

Example
```python
x=10
y=10
#both will be point to same memory reference
id(x) # print the memory location
x is y # is operator used to find does in same location
```

## Namespace

A namespace is a mapping from names to objects. It serves as a container for identifiers (variable names, function names, class names, etc.) and helps in avoiding naming conflicts and organizing code logically.

**Local Namespace**:
- A local namespace contains the names that are defined within a specific function or method.
- It is created when a function is called and is destroyed when the function exits.
- The local namespace is accessible only within the function or method where it is defined.

**Enclosing Namespace (non-local)**:
- An enclosing namespace contains names defined in the enclosing function or outer scope.
- It is used in nested functions or closures. use keyword `non-local`
- `Names in the enclosing namespace are accessible within the nested function but not modifiable by it.`

**Global Namespace**:
- The global namespace contains names defined at the top level of a module or script.
- It is created when the module is imported or when the script is executed.

**Built-in Namespace**:
- The built-in namespace contains names of built-in functions, exceptions, and other objects provided by Python.
- It is automatically loaded when Python starts up.
- The objects in `__builtins__` include commonly used functions like `print()`, `len()`, `range()`, etc
- Python 3, `__builtins__` is read-only by default.

```python

print(__builtins__.len([1, 2, 3])) # Output: 3

```

**Vars**

In Python, the `vars()` function is used to return the `__dict__` attribute of an object, which contains all the writable attributes of that object in the form of a dictionary. If you call `vars()` without any arguments, it returns the `__dict__` of the current local scope.

```python
def example_function():
    a = 10
    b = 20
    print(vars())  # This will show {'a': 10, 'b': 20}

example_function()

class MyClass:
    def __init__(self):
        self.x = 5
        self.y = 10

obj = MyClass()
print(vars(obj))  # This will show {'x': 5, 'y': 10}

```
## input and output 

### Reading Input:

```python
# Reading input from the user
user_input = input("Enter something: ")

# Print the input
print("You entered:", user_input)
```

The `input()` function takes an optional string argument that serves as the prompt, which will be displayed to the user before waiting for input. It returns the user's input as a string.

### Output:

```python
# Printing output
print("Hello, world!")
```

The `print()` function is used to display output to the console. You can pass one or more objects as arguments to `print()`, and they will be printed to the console separated by spaces by default.

### Formatting Output:

You can also format the output using various techniques such as string formatting or using the `.format()` method:

```python
name = "Alice"
age = 30
print("Name: {}, Age: {}".format(name, age))
# or
print(f"Name: {name}, Age: {age}")
```


### Conditional Statements:

#### `if-elif-else` statement:
The `if-elif-else` statement is used when there are multiple conditions to check.

```python
x = 10
if x > 5:
    print("x is greater than 5")
elif x == 5:
    print("x is equal to 5")
else:
    print("x is less than 5")
```

### Loops:

#### 1. `for` loop:
The `for` loop is used to iterate over a sequence (such as a list, tuple, string, etc.) or any iterable object.

```python
for i in range(5):
    print(i)
```

#### 2. `while` loop:
The `while` loop is used to execute a block of code repeatedly as long as a condition is true.

```python
x = 0
while x < 5:
    print(x)
    x += 1
```

#### Loop Control Statements:

##### - `break` statement:
The `break` statement is used to exit the loop prematurely based on a condition.

```python
for i in range(10):
    if i == 5:
        break
    print(i)
```

##### - `continue` statement:

```python
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)
```

## Functions

```python
def function_name(args):
	body
```

function will default return none
#### _arbitrary positional and Arbitrary Keyword arguments_

**Positional** : When we define a parameter with `*args`, it allows the function to accept any number of positional arguments. These arguments are packed into a tuple inside the function

**Keyword:** it allows the function to accept any number of keyword arguments. These arguments are packed into a dictionary inside the function.

```python
#**Positional**

def my_function(*args):
    for arg in args:
        print(arg)

my_function(1, 2, 3, 4)

#Keyword
def my_function(**kwargs):
    for key, value in kwargs.items():
        print(key, ":", value)

my_function(a=1, b=2, c=3)

```

#### Unpacking arguments

To pass elements of a list, tuple, or dictionary as separate arguments to a function.

```python
   def my_function(a, b, c):
       print("a:", a)
       print("b:", b)
       print("c:", c)

   my_list = [1, 2, 3]
   my_function(*my_list)
   
   my_dict = {'a': 1, 'b': 2, 'c': 3}
   my_function(**my_dict) 
   # the function params need to have same name as keys in dict
```

#### **Lambda Functions**

Lambda functions, also known as anonymous functions,

```python
lambda arguments: expression
```

Lambda functions are typically used when you need a simple function for a short period, often as an argument to higher-order functions like `map()`, `filter()`, and `sorted()`, or within a list comprehension.

```python
add = lambda x, y: x + y
print(add(3, 5))  # Output: 8
```

## Generators

Generators are functions that can pause and resume their execution. They generate a sequence of values lazily, one value at a time, and only when requested.

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# Using the generator in a loop
for i in countdown(5):
    print(i)
    

gen = countdown(3)

print(next(gen))  # Output: 3
print(next(gen))  # Output: 2

```

**Memory Efficiency**: Generators produce values on-the-fly, so they don't store the entire sequence in memory at once. This makes them memory efficient, particularly for large datasets.

**Lazy Evaluation**: Values are generated only when requested, which can improve performance and reduce unnecessary computation.
   
**Support for Infinite Sequences**: Generators can produce infinite sequences of values without running out of memory or crashing the program.

#### Decorators

Allows us to modify or extend the behavior of functions or methods. They provide a clean and concise way to add functionality to existing code without modifying it directly.
```python

def my_decorator(func):
    def wrapper():
        print("Before")
        func()
        print("After")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()

#output
Before
Hello! 
After
```

When we give multiple decorator it will excute from bottom to top
## Modules

When a Python module is imported, it gets its own `__name__` attribute. This attribute contains the name of the module. Additionally, modules have a `__file__` attribute, which stores the path to the module's source file.

```python
import my_module 
import my_module as mm #**Module Aliases**
from my_module import my_function` #**Importing Specific Items**
```

When Python executes a script, it assigns the special variable `__name__` a value of `"__main__"` if the script is being run directly. By using `if __name__ == "__main__":`, we can write code that will only execute when the script is run directly and not when it's imported as a module.

```python
# my_script.py

def main():
    print("This is the main function.")

if __name__ == "__main__":
    main()

```

#### Import module resolution

 **sys.path**: Python maintains a list of directories called `sys.path` where it searches for modules when you import them. This list includes several default locations such as:
 - The directory containing the script that is being executed.
- The directories listed in the `PYTHONPATH` environment variable.
- The installation-dependent default path, which typically includes standard library directories, site-packages directories, etc.

**site-packages Directory**: When you install third-party packages using tools like `pip`, they are typically installed into a directory called `site-packages`. This directory contains modules and packages installed via `pip` and is added to `sys.path`.

```python
dir(modulename) #print all function present in module
print(help(modulename))
```


```python
#my_package/pythonscript.py

from my_package import pythonscript #will import the whole file
pythonscript.myfunc()

from my_package.pythonscript import myfunc
```

Let say if we have two file inside single folder if we want to import both file like `from foldername import *` it won't work we need to use package by creating the `__init__` file 
## Packages

A package is a collection of Python modules organized in a directory hierarchy. It typically contains an `__init__.py` file to indicate that the directory should be treated as a package. Packages allow you to further organize your code into meaningful units.

```python
# my_package/__init__.py

# Initialization code for the package
print("Initializing my_package")

# Importing specific functions or classes from the package modules
from .module1 import function1
from .module2 import function2

# Defining __all__ to specify what is exported when 'from my_package import *' is used
__all__ = ['function1', 'function2']

```

#### Wheel files

is a built package format for Python that helps distribute and install Python software. It is a binary distribution format, which means it contains all the files necessary for running the package without requiring a build step. This format makes installation faster and easier compared to source distributions, which need to be built on the user's system.

when we installing pkg using pip it get downloaded as wheel file.

### Wheel File Structure

A wheel file is essentially a ZIP archive with a specific naming convention and structure. The structure typically includes:

- `data/`: Optional directory for package data files.
- `dist-info/`: Directory containing metadata files such as `METADATA`, `RECORD`, `WHEEL`, and `entry_points.txt`.
- `name/`: The package’s code files.
- 
#### __pycache__

`__pycache__` is a directory automatically created by Python when it compiles source files into bytecode files. Python compiles source files into bytecode (.pyc) files to speed up execution by avoiding recompilation each time the script is run.

1. When you import a module in Python, the interpreter first checks if there's a corresponding bytecode file (.pyc) in the `__pycache__` directory.
2. If the bytecode file exists and is up to date (i.e., the source file has not been modified since the bytecode was generated), Python uses the bytecode file for execution, bypassing the compilation step.
3. If the bytecode file does not exist or is outdated, Python compiles the source file into bytecode and stores it in the `__pycache__` directory for future use.

## Context manger

objects that are used with the `with` statement to ensure that certain operations are properly initialized and cleaned up

```python
class MyContextManager:
    def __enter__(self):
        # Initialization code goes here
        print("Entering the context")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        # Cleanup code goes here
        print("Exiting the context")
        # Handle exceptions if needed
        return False  # True if exceptions are handled, False otherwise

# Using the context manager with the 'with' statement
with MyContextManager() as cm:
    # Code inside the 'with' block
    print("Inside the context")

```
## Builtin modules


## Exception handling

```python
try:
    # Code that might raise an exception
    x = 10 / 0
except Exception as e: #catch all exception
    print("Cannot divide by zero!")
 # Handle the specific exception (ZeroDivisionError in this case)
except ZeroDivisionError: 
    print("Cannot divide by zero!")
# Handle division by zero print("Cannot divide by zero!")

finally:
    # Optional cleanup code
    print("This will always execute.")

```
 
 **Custom erros:**
```python

# Define a custom exception class
class CustomError(Exception):
    def __init__(self, message="Something went wrong."):
        self.message = message
        super().__init__(self.message)

# Raise the custom exception
def example_function(x):
    if x < 0:
        raise CustomError("Input value cannot be negative.")
        #or
        # raise Exception("Input value cannot be negative.") 

try:
    example_function(-5)
except CustomError as e:
    print("Custom error caught:", e)

```
## OOPS

   ```python
   class Car:
       def __init__(self, brand, model):
           self.brand = brand #attributes
           self.model = model

       def drive(self):
           print(f"Driving {self.brand} {self.model}")

   my_car = Car("Toyota", "Camry")
   my_car.drive()  # Output: Driving Toyota Camry
   ```

 **Attributes**: Attributes are variables associated with a class or an object. They represent the state of the object. methods of attributes `hasattr()` , `setattr()`, `delattr()` and `getattr()`
 
**Methods**:Methods are functions defined within a class that operate on the object's data. The first argument of a method is always `self`, which refers to the object itself.

**Constructor (`__init__`)**: The `__init__` method is a special method called the constructor. It is used to initialize object attributes when an object is created.

**Inheritance** Inheritance allows a class (subclass) to inherit attributes and methods from another class (superclass). Subclasses can override or extend the behavior of the superclass.

```python
   class ElectricCar(Car):
       def __init__(self, brand, model, battery_capacity):
           super().__init__(brand, model)
           self.battery_capacity = battery_capacity

       def charge(self):
           print(f"Charging {self.brand} {self.model} with {self.battery_capacity} kWh")

   my_electric_car = ElectricCar("Tesla", "Model S", 100)
   my_electric_car.drive()   # Output: Driving Tesla Model S
   my_electric_car.charge()  # Output: Charging Tesla Model S with 100 kWh
```

**Class (static) variable**: are variables that are shared among all instances (objects) of a class. They are defined within a class but outside of any methods. Class variables are accessed using the class name rather than instance variables

**Static method** : It is decorated with `@staticmethod` to indicate that it is a static method. can be called using class name or using instance. static methods don't have access to instance attributes or methods

**Class Methods**: They have access to class variables but not to instance variables.The first parameter of a class method conventionally named `cls`, which refers to the class itself.

```python
class Dog:
    # Class variable
    species = 'Canis familiaris'

    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age
	
	@staticmethod 
	def static_method(): 
		print("This is a static method")
	
	@classmethod 
	def class_method(cls): 
		print("Class method called")

# Accessing class variable using class name
print(Dog.species)  # Output: Canis familiaris

# Creating instances of the Dog class
dog1 = Dog('Buddy', 3)
dog2 = Dog('Max', 5)

# Accessing class variable using instance
print(dog1.species)  # Output: Canis familiaris
print(dog2.species)  # Output: Canis familiaris

# Modifying class variable using class name
Dog.species = 'Canis lupus'  # Changes species for all instances

# Accessing class variable using instance after modification
print(dog1.species)  # Output: Canis lupus
print(dog2.species)  # Output: Canis lupus
print(hasattr())
```

**Dunder methods**: are special methods in Python that have double underscores at the beginning and end of their names. These methods allow you to define behavior for built-in operations in Python. 

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Point({self.x}, {self.y})"
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)

# Create two Point objects
p1 = Point(1, 2)
p2 = Point(3, 4)

# Test magic methods
print(p1)          # Output: (1, 2)
print(repr(p1))    # Output: Point(1, 2)
print(p1 == p2)    # Output: False
print(p1 + p2)     # Output: Point(4, 6)
print(p2 - p1)     # Output: Point(2, 2)

```

## File handling

```python
# Open a file for reading
file = open("example.txt", "r")

# Open a file for writing
file = open("example.txt", "w")

# Open a file for appending
file = open("example.txt", "a")

# Read the entire file
content = file.read()

# Read one line at a time
line = file.readline()

# Read all lines into a list
lines = file.readlines()

# Write to a file
file.write("Hello, World!")

# Close the file 
file.close()

#using context manager
# Open a file for writing
with open("example.txt", "w") as file:
    file.write("Hello, World!\n")
    file.write("This is a Python file handling example.\n")

```

#### File Modes
- `"r"`: Read mode (default). Opens a file for reading. File must exist.
- `"w"`: Write mode. Opens a file for writing. Creates a new file or truncates an existing file.
- `"a"`: Append mode. Opens a file for appending. Creates a new file if it does not exist.
- `"b"`: Binary mode. Used in conjunction with other modes to open a file in binary mode (e.g., `"rb"`, `"wb"`, `"ab"`).

## Threading

create a new thread by subclassing the `Thread` class and overriding the `run()` method or by passing a target function to the `Thread` constructor.
```python
import threading

# Subclassing Thread class
class MyThread(threading.Thread):
    def run(self):
        print("Thread is running")

# Using target function
def my_function():
    print("Thread is running")

thread1 = MyThread()
thread2 = threading.Thread(target=my_function)

thread1.start() 
thread2.start()
#### Waiting for Threads to Complete
thread1.join()
thread2.join()
```

**Join**: `join()` method is used to wait for a thread to complete its execution before continuing with the main thread. When you call `join()` on a thread object, the program will block and wait until that thread finishes its execution.`If we want the main thread to wait for one or more additional threads to finish their execution before proceeding further, you can call the `join()` method on those thread objects.`

**Daemon thread**: Are threads that run in the background and are automatically terminated when the program exits it not like normal threads

```python
import threading
import time

def demon_worker():
    while True:
        print("Demon thread is running...")
        time.sleep(1)

# Create a demon thread
demon_thread = threading.Thread(target=demon_worker)
demon_thread.daemon = True  # Set the thread as demon

# Start the demon thread
demon_thread.start()

# Main thread continues execution
print("Main thread continues...")

# Simulate the program running for a while
time.sleep(5)

```
## Byte code

Generated by the Python interpreter when it translates the source code into a form that can be executed by the Python virtual machine (PVM).
```python
import dis

def add(a, b):
    return a + b

# Use the disassemble function from the dis module to inspect the bytecode
dis.dis(add)
```

```bytecode
  4           0 LOAD_FAST                0 (a)
              2 LOAD_FAST                1 (b)
              4 BINARY_ADD
              6 RETURN_VALUE
```


## Async IO

`asyncio` module provides infrastructure to write single-threaded concurrent code using coroutines, multiplexing I/O access over sockets and other resources, running network clients and servers, and other related primitives.

1. **Coroutines (`async`/`await`)**: Coroutines are special functions that can pause execution at `await` expressions and then resume where they left off. They are defined using `async def` syntax and can be paused and resumed asynchronously.
   
2. **Event Loop**: The event loop is the central component of `asyncio`. It manages and schedules coroutines, IO operations, and callbacks. The event loop continuously checks for events such as IO operations, callbacks, and scheduled tasks, and executes them in an asynchronous manner.
  
3. **Tasks**: Tasks are used to schedule coroutines for execution on the event loop. A task wraps a coroutine, allowing it to be scheduled and managed by the event loop.

4. **Event Loop Policies**: Event loop policies allow you to customize the event loop used by `asyncio`. This can be useful for integrating `asyncio` with other event loops or frameworks.

```python
import asyncio

# every async function return co-routine
async def hello():
    print("Hello")
    await asyncio.sleep(1) #do some async operation
    print("World")

async def main():
    await hello()

asyncio.run(main())

# similar to promise.all
result  = await asyncio.gather(*[hello(),hello()]) 
# or 
# await asyncio.gather(hello(),hello()) 

# async iteration
async for i in async_function():
	print(i)

#task

# `asyncio.to_thread` to run blocking code in a separate thread without blocking the event loop


def blocking_task(n):
    print(f'Starting blocking task {n}')
    time.sleep(2)  # Simulate a delay
    print(f'Finished blocking task {n}')
    return f'Result from task {n}'

async def main():
    tasks = []
    
    # Use asyncio.to_thread to run blocking tasks concurrently
    for i in range(5):
        task = asyncio.to_thread(blocking_task, i)
        tasks.append(task)

    # Wait for all tasks to complete and gather results
    #if one failes it won't cancel other task
    results = await asyncio.gather(*tasks)
    
    # Print results
    for result in results:
        print(result)


```

**Coroutine**

A **coroutine** is a special type of function in Python (and other programming languages) that allows you to write asynchronous code. Unlike regular functions, coroutines can pause their execution to let other code run, making them ideal for tasks that involve waiting, such as network requests or file I/O.

coroutine starts running when it is awaited or scheduled to run within an event loop.

```python
async def my_coroutine():
    print("Coroutine started")
    await asyncio.sleep(1)  # Simulate an async operation
    print("Coroutine ended")

async def main():
     my_coroutine()  # we not put await so the code will not be excuted

asyncio.run(main())

```

**Task**

task is a wrapper for a coroutine that allows it to run concurrently with other coroutines. if we use await it wil run one by one which is same as single thread.

we  can create tasks using `asyncio.create_task()` or `loop.create_task()`

Not like `gather` it will cancel other task on error 

```python

async def my_coroutine(n):
    print(f'Starting task {n}')
    await asyncio.sleep(2)  # Simulate a non-blocking delay
    print(f'Finished task {n}')
    return f'Result from task {n}'

async def main():
    # Create a list of tasks
    tasks = []
    for i in range(5):
        task = asyncio.create_task(my_coroutine(i))
        tasks.append(task)

    # Wait for all tasks to complete and gather results
    results = await asyncio.gather(*tasks)

    # Print results
    for result in results:
        print(result)

# Run the main function in the event loop
if __name__ == '__main__':
    asyncio.run(main())

```

Error handling in task and gather
- `return_exceptions` argument to return exceptions as part of the results instead.

```python
async def main():
    tasks = [task_with_error(i) for i in range(5)]
    results = await asyncio.gather(*tasks, return_exceptions=True)

    for result in results:
        if isinstance(result, Exception):
            print(f"Caught an exception: {result}")
        else:
            print(result)

# task 

async def main():
    task = asyncio.create_task(task_with_error(2))  # This will raise an error
    try:
        result = await task
    except Exception as e:
        print(f"Caught an exception from the task: {e}")

```

### Under the Hood of async IO

asyncio event loop is written in C
```python
from queue import Queue

event_loop = Queue()

class Task():
	def __init__(self, generator):
		self.iter = generator		
		self.finished = False

	def done(self):
		return self.finished

	def __await__(self):
		while not self.finished:
			yield self


def create_task(generator):
	task = Task(generator)
	event_loop.put(task)
	return task

def run(main):
	event_loop.put(Task(main))
	while not event_loop.empty():
		task = event_loop.get()
		try:
			task.iter.send(None)
		except StopIteration:
			task.finished = True
		else:
			event_loop.put(task)
```
## Nest async io

`nest_asyncio` is a Python library that allows you to run an asyncio event loop within an already running event loop. This is particularly useful in environments like Jupyter notebooks or other interactive environments where an event loop may already be running.

```python

import nest_asyncio
import asyncio

# Apply the patch to allow nesting of the asyncio event loop
nest_asyncio.apply()

async def say_hello():
    print("Hello!")
    await asyncio.sleep(1)
    print("Goodbye!")

async def main():
    await say_hello()

if __name__ == '__main__':
    asyncio.run(main())

```

Resources
- https://jacobpadilla.com/articles/handling-asyncio-tasks
## Type hints & Annotations

Introduced in python 3.5 above it won't force to strict type we can give any type

```python

x:str =1 #no error

def func(a:str,b:str) -> str :
	return a

from typing import List,Dict,Set,Optional,Any,Sequence,Callable,TypeVar

list : List[List[int]] = [[1,2,3]]

dict : Dict[str,str] = {"key":"vaule"}


# Reusing type by assign to var
Vector = List[float]

list : Vector = [1.3,2.4]

def func (a:Optional[str],b:Any)

#func as param first array args and last one is return type

def func(func:Callable[[int,int],[int]]):


#Generic
T = TypeVar("T")

def func(list:List[T])->T :
	return ""


```

if we want to force type we need to use static analysis of code using `pip install mypy`

```python

mypy filename.py #will strictly check the type
```


## Other Modules

### Pydantic
Pydantic is the most widely used data validation library for Python.

```python

from datetime import datetime
from typing import Tuple

from pydantic import BaseModel


class Delivery(BaseModel):
    timestamp: datetime
    dimensions: Tuple[int, int]


m = Delivery(timestamp='2020-01-02T03:04:05Z', dimensions=['10', '20'])
print(repr(m.timestamp))
#> datetime.datetime(2020, 1, 2, 3, 4, 5, tzinfo=TzInfo(UTC))
print(m.dimensions)
#> (10, 20)

```

## Docstrings

A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition. It is used to document the code and provide information about what the code does, its parameters, return values, and any other relevant details.

A docstring is typically enclosed in triple quotes `"""..."""` or `'''...'''`, and it can span multiple lines.

```python
def greet(name: str) -> None:
    """Print a personalized greeting message.

    Args:
        name (str): The name of the person to greet.

    Returns:
        None
    """
    print(f"Hello, {name}!")

print(greet.__doc__) 
help(greet)
```


## Package mangement

In Python, package management and package locking are handled using tools that manage dependencies and ensure consistency across environments. The main tools that provide functionality similar to `package.json` and `package-lock.json` in JavaScript are:

### 1. **`requirements.txt` + `pip`**
   - **`requirements.txt`** is a simple text file used to specify the exact versions of the Python packages your project depends on. 
   - **`pip`** is the package manager that installs and manages these packages.
   
   To create a `requirements.txt` file:
   ```bash
   pip freeze > requirements.txt
   ```
   To install packages from it:
   ```bash
   pip install -r requirements.txt
   ```
   However, this setup doesn’t lock exact versions for sub-dependencies (like `package-lock.json` does).

### 2. **`pipenv`** 
   - **`Pipenv`** is a modern Python packaging tool that creates two files:
     - `Pipfile`: Defines project dependencies.
     - `Pipfile.lock`: Locks the exact versions, including transitive dependencies, ensuring consistency across environments.
   
   Commands:
   - Install a package: 
     ```bash
     pipenv install <package_name>
     ```
   - Generate `Pipfile` and `Pipfile.lock` and install dependencies:
     ```bash
     pipenv install
     ```

### 3. **`poetry`**
   - **`Poetry`** is another modern package manager that also handles dependency management and version locking.
     - `pyproject.toml`: Similar to `package.json`, it contains dependency specifications.
     - `poetry.lock`: Similar to `package-lock.json`, it locks all package versions and their dependencies.
   
   Commands:
   - To install and manage dependencies:
     ```bash
     poetry install
     ```
   - Add a package:
     ```bash
     poetry add <package_name>
     ```

### 4. **conda (for Anaconda users)**
   - **`Conda`** is a package manager for Python and other languages, typically used in scientific computing.
   - `environment.yml`: Similar to `package.json`, this file defines all packages and their versions, along with specific channels.
   - You can export a lock file using:
     ```bash
     conda env export > environment.yml
     ```

- https://dublog.net/blog/so-many-python-package-managers/
### Comparison:

| Tool     | Dependency File    | Lock File      | Environment Isolation | Notes                                        |
| -------- | ------------------ | -------------- | --------------------- | -------------------------------------------- |
| `pip`    | `requirements.txt` | No Lock File   | No                    | Most basic setup                             |
| `pipenv` | `Pipfile`          | `Pipfile.lock` | Yes                   | Modern solution                              |
| `poetry` | `pyproject.toml`   | `poetry.lock`  | Yes                   | Great for projects with complex dependencies |
| `conda`  | `environment.yml`  | No Lock File   | Yes                   | Popular for data science projects            |


new tools
- pydantic
- Ruff [ linting and formating]
- uv (An extremely fast Python package and project manager, written in Rust.) Good need to try this
- mypy



### PEP

PEP stands for **Python Enhancement Proposal**. It is a design document providing information or proposing new features, processes, or changes to Python. PEPs serve as the primary mechanism for discussing, documenting, and communicating ideas to the Python community.

**Types of PEPs**

1. **Standards Track PEPs**: Propose new features or standards to be added to Python. 
	- Example: **PEP 8** (Style Guide for Python Code), **PEP 484** (Type Hints).
1. **Informational PEPs**: Provide guidelines or general information to the Python community.
	- Example: **PEP 20** (Zen of Python).
1. **Process PEPs**: Propose changes to the development process or tools for Python.
	- Example: **PEP 1** (PEP Purpose and Guidelines).