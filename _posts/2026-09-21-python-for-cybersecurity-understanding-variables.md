---
title: Python for Cybersecurity - Understanding Variables
image:
  path: /assets/2026-09-21-python-for-cybersecurity-understanding-variables/483c389b-8a74-4459-b3a9-676994d758e9.jpg
date: 2026-09-21 06:00:00 -0600
categories: [Programming Languages, Python]
tags: [python, python-fundamentals]     # TAG names should always be lowercase
---

A **variable** is simply a named place where you store a piece of information so you can use it later.

**Variables** are the fundamental building blocks of programming, and you'll find them virtually in every programming language. 

In cybersecurity, for instance, you can use them to keep track of information such as an IP address, a network port, a protocol, etc. 

Here's a simple example: 

```python
target_ip = "192.168.56.101"
target_port = 443
protocol = "HTTPS"
is_encrypted = True
```

On the left, you have the variable itself, represented by a clear and descriptive name: 

```python
target_ip
target_port
protocol
is_encrypted
```

On the right, you've got the value that you intend to store in the variable: 

```python
"192.168.56.101"
443
"HTTPS"
True
```

The `=` sign is the **assignment operator** and it tells Python to assign the _value_ on the right to the _variable_ on the left. As simple as that! 🫠

What's even cooler is that Python lets you assign multiple variables in the same line, all in one go:

```python
target_ip, target_port, protocol, is_encrypted = "192.168.56.101", 443, "HTTPS", True
```

How dope is that? 😎

**Variables** will become one of the tools you use constantly. They'll let you keep track of information about a target IP, ports, connections settings, scan results, and much more. 

## Data Types

Python is smart enough to figure out the **type of data** you're storing in a variable.

For example, take a look at these variables:

```python
# string 
protocol = "HTTPS"

# integer 
target_port = 443 

# float 
scan_timeout = 2.5 

# boolean
is_encrypted = True
```

Here, Python automatically recognizes that `target_port` contains an integer, `scan_timeout` a decimal number, and `protocol` contains text. You don't have to explicitly tell Python which type you're using, it figures it out from the value you assign.

However, if you're ever unsure about the type of a variable, you can always use the Python's built-in `type()` function:

```python
protocol = "HTTPS"
target_port = 443 
scan_timeout = 2.5 
is_encrypted = True

print(type(protocol))      # <class 'str'>
print(type(target_port))   # <class 'int'> 
print(type(scan_timeout))  # <class 'float'> 
print(type(is_encrypted))  # <class 'bool'>
```

The `type()` function tells you what kind of data a variable currently contains.

### `None` - When There's No Value

So far, you've seen values such as _integers_, _decimal numbers_, _strings_, and _booleans_. But, what if you need a variable that doesn't have a value yet?

Well, that's where `None` comes into play.

You see, `None` is a special Python value that represents the **absence of a value** (I know, it might sound a little contradictory, but bear with me). Imagine you're building a tool that will eventually determine whether a target is reachable:

```python
target_ip = "192.168.56.101"
is_reachable = None
```

Your first instinct might be initialize `is_reachable` with `False`,  but that would mean the target isn't reachable.

At this point, you haven't performed the check yet, you don't know whether the target is reachable or not. That's why `None` is a better initial value for `is_reachable`: 

```python
is_reachable = None
```

It communicates that you don't know the answer yet. Later, after your program performs the check, `is_reachable` can be updated accordingly either to `True` or `False`, reflecting the actual result of the check.

And lastly, just like the other data types, you can also use the built-in `type()`  function on `None` to find out what type it belongs to: 

```python
is_reachable = None

print(type(is_reachable)) # <class 'NoneType'>
```

As you can see, `None` has its own data type called `NoneType`.

## Variable Mutation 

Such a fancy term for something pretty simple: **reusing an existing variable name by assigning it a new value**.

You see, as your program grows, you might find yourself replacing a variable's current value with something else. That's completely normal, that's the reason why they are called variables in the first place, so:

```python
target_port = 443

# At some point later in your code...
target_port = 80
```

Nothing wrong here.

But what happens if the new value is of a completely different type?: 

```python
target_port = 443 

# At some point later in your code... 
target_port = "443"
```

At first glance, this might look harmless. Python won't complain, and your program continues running, but problems can appear later on when you try to perform an operation that expects a number, for example:

```python
target_port = 443 

# At some point later in your code... 
target_port = "443"

# Try to calculate a new port number 
target_port += 1
```

Python will throw a `TypeError: can only concatenate str (not "int") to str` at you because you're trying to add an integer to a string. From Python's perspective, this operation simply doesn't make any sense for those two types and that’s the reason the code fails.

Variables inherently change their values over time, that's perfectly normal. However, **it’s important to distinguish what value you're assigning to a variable and what type of data it contains as your code progresses**.

## User-defined Constants

A **constant**, as the name suggests, represents a value that doesn’t change overtime. Real-world examples of constants include the _speed of light_, the _freezing point of water_, and the mathematical constant `π`.

In Python, constants are conventionally declared using only **upper letters**. For example:

```python
DEFAULT_PORT = 443 
CONNECTION_TIMEOUT = 5 
MAX_RETRIES = 3
```

While a _variable_ can change during its lifetime, a **constant** cannot. Once you assign a value to a constant, you can read from it but you cannot change its value, well at least in Python, that's expected of you since Python don't enforce read-only access. The convention is just there to convey the idea that you, the developer, know that this variable is a constant, and it shouldn’t be modified.

### Constants and Magic Numbers

Another useful reason to use constants is to replace **magic numbers**.

You see, a **magic number** is simply a literal numeric value that appears in your code without making its meaning obvious. For example: 

```python
if connection_attempts > 3:
    print("Too many connection attempts")
```

Can you tell what is the meaning of the numeric value `3`?, maybe you can, probably you cannot. To get rid of this ambiguity you can use a constant:

```python
MAX_RETRIES = 3 

if connection_attempts > MAX_RETRIES:
	print("Too many connection attempts")
```

Now anyone reading the code immediately knows what `3` represents.

This small change makes your code easier to **read, understand, and maintain**. If you later decide that the maximum should be `5` instead of `3`, you only need to change it in one place.

As you start building cybersecurity tools, you'll encounter plenty of values that make sense as constants: _connection timeouts_, _retry limits_, _default ports_, _protocol values_, _buffer sizes_, and other configuration values.

## Python Comments

If you take a closer look at the examples I’ve provided throughout the article you’ll notice lines of code starting with the hash symbol (`#`). Well, these are **comments**. 

Comments are there for you (and other developers) to explain what's happening in your code. For example: 

```python
# Default HTTP port used by the target
target_port = 443
```

Keep in mind that Python will ignore comments when executing your program. They are just there to make your code easier for other humans to understand.  

### Writing Multiple Lines of Comments

You can also comment your code using multiple lines by stacking one comment on top of another, like so: 

```python
# Store information about the target. 
# We'll use these values later when 
# performing network reconnaissance.
```

### Docstrings

Another way of writing multi-line comments is by using documentation strings, formally known as **Docstrings**. 

Docstrings are strings specifically used to document code. They are especially important when you're building functions, classes, and modules because they can explain what a piece of code does, what information it expects, and what it returns.

Docstrings are typically written using triple quotation marks:

```python
""" 
This module contains tools for gathering 
basic information about a network target. 
"""
```

You'll get even more value from docstrings once you start working with functions. For now, just remember that they're designed specifically for documenting larger pieces of code.

### Single-line Comments vs. Docstrings

Although comments and docstrings both help document your code, they serve different purposes.

**Comments** are useful for explaining specific lines or decisions in your code:

```python
# Use HTTPS when communicating with the target 
protocol = "HTTPS"
```

**Docstrings**, on the other hand, are used to document larger pieces of code, such as modules, functions, or classes: 

```python
def scan_target(target_ip): 
    """ Perform a basic scan against the specified target IP. """
```

In the end, both are valuable when writing cybersecurity tools. As your program becomes larger and more complex, comments and docstrings will help keep your code readable and easier to understand. They'll provide clarity and useful context explaining why a certain piece of code behaves the way it does.

## Good Practices

They say one of the hardest things in programming is to come up with good names, I agree.

When naming variables, I recommend that you:

1. Make them **pronounceable**.
2. Make them **reveal their intention**. 
3. Avoid unnecessary abbreviations. Common, well-understood abbreviations such as `ip`, `tcp`, or `url` are fine.
4. Make them **long enough to describe the concept clearly**, but not so long that they become difficult to read.
5. Consider the context in which the variable is defined and used.
6. Differentiate between **nouns** and **verbs**.

For example, compare this:

```python
x = "192.168.56.101" 
p = 443 
t = 5
```

With this: 

```python
target_ip = "192.168.56.101" 
target_port = 443 
connection_timeout = 5
```

Both examples work, but the second one makes the purpose of each variable immediately obvious.

And remember: **understanding the purpose of a variable is way more important than just figuring out its type**.

### Keep Sensitive Information Out of Your Code

When writing your cybersecurity tools, please **never hardcode sensitive information int your source code** 😖.

Things such as passwords, API keys, authentication tokens, private keys, and other secrets should not be sitting directly inside your Python files:

```python
api_key = "my-super-secret-api-key" 
password = "P@55w0rd123"
```

The problem with this is that your code can end up in places you didn't expect, like a Git repository, a log file, a backup, or even a screenshot. 

Later in this course, you'll learn safer ways to handle sensitive information, such as using environment variables and configuration files. For now, remember this simple rule:  **If it's a secret, don't put it directly in your source code**.

### Follow Python's Conventions

Finally, while there are various conventions and guidelines out there for writing Python code, I find [PEP-8](https://peps.python.org/pep-0008) particularly appealing for crafting elegant and readable Python code.

**PEP 8** covers things like _naming conventions_, _indentation_, _whitespace_, _line length_, and overall _code layout_. Following these conventions will make your code more consistent and easier for other Python developers to read.

## Challenge Time - Build a Basic Network Recon Profile

You've just joined a security team as a junior security engineer. Your team is conducting an authorized security assessment of a server in an isolated sandbox environment. 

Before your reconnaissance tool can communicate with the target, it needs to know what it's working with. Your first task is to create a simple profile that stores the target's information.

Your team has provided you with the following information: 

- **Hostname:** `web-server-01`
- **IP address:** `192.168.56.101`
- **Operating system:** `Linux`
- **Primary service:** `HTTPS`
- **Port:** `443`
- **Connection encrypted:** `True`
- **Network timeout:** `5` seconds
- **Maximum number of retries:** `3`

Your task is to create a script that produces a readable output similar to:

```
=== Network Recon Target ===

Hostname: web-server-01
IP Address: 192.168.56.101
Operating System: Linux
Primary Service: HTTPS
Port: 443
Encrypted: True
Timeout: 5 seconds
Maximum Retries: 3
```

Your code should contains at least strings, integers, a boolean, and a constant. Use meaningful and descriptive variable names. Use the `print()` function to display the information. 

Don't worry if your solution doesn't look exactly like someone else's. There are often multiple valid ways to represent the same information in Python.

As you learn more Python, we'll return to this project and give it new capabilities. Keep building, there's much more to come!. 🫨
