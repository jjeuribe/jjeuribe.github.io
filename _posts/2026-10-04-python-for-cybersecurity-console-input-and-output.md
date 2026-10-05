---
title: Python for Cybersecurity - Console Input and Output
image:
  path: /assets/2026-10-04-python-for-cybersecurity-console-input-and-output/ywb4jlywb4jlywb4.jpg
date: 2026-10-04 06:00:00 -0600
categories: [Programming Languages, Python]
tags: [python, python-fundamentals]     # TAG names should always be lowercase
---

Think about a port scanner: before it can scan anything, it needs information such as a target host, a range of ports, and some scanning options. Once the scan is complete, it communicates the results, perhaps by displaying information about open ports in the terminal or dumping them to a file for further analysis.

Now think about a log analyzer. It consumes log entries and produces relevant events. An enumeration script receives a target, gathers information from it, and presents its findings to the analyst.

Can you see the pattern? 🤔

Yes, a cybersecurity tool, just like any other program, _receives_, _processes_, and _produces data_. This is where **input and output** become fundamental concepts.

**Input** is any data that enters your program. It can come from different sources such as the keyboard, command-line arguments, files, databases, networks connections, APIs, or even another program.

**Output** is the information your program produces. It can be displayed in the terminal or a graphical interface, written to a file, or generated into a structured format such as JSON.

In this article, you'll learn Python's basic input and output mechanisms, fundamental skills that will  become really useful for you as you begin building more practical cybersecurity tools.

## Reading Input from the Keyboard with `input()`

Python provides you the `input()` function to collect information typed by the user: 

```python
target_host = input("Enter the target to scan: ")

print("Target acquired: " + target_host)
```

When this code runs, Python will pause for a moment and wait for the user to enter a value. Whatever the user types is then assigned to the `target_host` variable. **This is the simplest form of input**.

Keep in mind though, that `input()` **always returns a string**. So, even if a user enters what looks like a different type of data, Python will receive it as a string.

For example, let's say the user enters the value `443`

```python
# User enters value 443
target_port = input("Enter the port to scan: ")

print(type(target_port)) # <class 'str'>
```

Python will simply treat the value as the string `"443"`.

If you want to convert that value into a different data type, consider using a **casting function** such as `int()` or `float()`:

```python
# User enters value 443
target_port = int(input("Enter the port to scan: "))

print(type(target_port)) # <class 'int'>
```

Now, `target_port` contains an integer, which makes it possible to perform numerical operations with it.

### Security Consideration: Never Trust User Input

When building your own cybersecurity tools, one principle should become second nature: **_"Never trust input simply because it came from a user"_**.

The `input()` function don't know whether the value provided by the user is correct or expected for what your program is about to do. It simply gives your program whatever the user entered.

So, if your tool expects a port number:

```python
target_port = input("Enter the port to scan: ")
```

Sure, you might expect the user to enter the correct information like `443`, but they could just easily enter the word `hello` or `443abc`, and Python will happily store it as a string. So, indeed, it is your responsibility to determine whether that input makes sense for the operation being performed by your program.

This is where concepts like **input validation** and **input sanitization** become really important. For example, before trying to use `target_port` as a port number, your program should verify that it contains the appropriate value.

We'll explore validation and other techniques for safely handling external data later in the course. For now, keep this simple principle in mind: **Never assume that data entering your program is correct. Make sure the input makes sense before you use it**.

## Spitting Out to the Console with `print()`

So far, you've seen one way at how a cybersecurity tool can receive information from a user. Now let's take a look at the other side of the equation: how a program can display information back to the user.

Python provides you the good well-known `print()` function for, well... **spitting information out to the console** 🐍

You can use it for example to report whether your target host is reachable: 

```python
print("Starting network scan...") 
print("Target: 192.168.1.10") 
print("Host is reachable")
```

The output: 

```
Starting network scan... 
Target: 192.168.1.10 
Host is reachable
```

Or, for your port scanner to report its findings: 

```python
print("Port 22: OPEN") 
print("Port 80: OPEN") 
print("Port 443: OPEN")
```

The result: 

```
Port 22: OPEN 
Port 80: OPEN 
Port 443: OPEN
```

A common use for `print()` is to display several pieces of information all at once. For example.

```python
target_host = "192.168.1.10" 
target_port = 443 
status = "OPEN" 

print(target_host, target_port, status)
```

Output: 

```
192.168.1.10 443 OPEN
```

When passing multiple values to `print()`, Python automatically inserts a space between them as you can see. This way, you can display the related pieces of information without having to create the entire string yourself. 

It's important to understand that by default, the `print()` function, adds a **newline character `\n`** to the end of its output. This means that each call to `print()`, Python displays your message and then moves the cursor to the beginning of the next line:

```python
print("Scanning target...")
print("Scan complete.") 
```

The output:

```
Scanning target... 
Scan complete.
```

However, this behavior can be modified as we'll see shortly.

### Printing Different Data Types

Another useful feature about `print()` is its ability to accept any data type and provide an appropriate string representation:

```python
target_port = 443 
host_is_up = True 
response_time = 0.153 
open_ports = [22, 80, 443] 
target = { "host": "192.168.1.10", "status": "up" }

print(target_port)   # 443 
print(host_is_up)    # True 
print(response_time) # 0.153 
print(open_ports)    # [22, 80, 443] 
print(target)        # {'host': '192.168.1.10', 'status': 'up'}
```

This is especially useful when debugging because it helps you figure out what's wrong with your code.

### Customizing Space Separation with `sep`

 You already know that by default, `print()` automatically places a space between the values when used with multiple arguments: 

```python
target_host = "192.168.1.10" 
target_port = 443 
status = "OPEN" 

print(target_host, target_port, status) # 192.168.1.10 443 OPEN
```

But what if you don't want to use a regular space? No problem, Python lets you customize this behavior: simply use the `sep` argument to tell `print()` exactly what you want to insert between the values. 

For example, your security tool might use a vertical bar to make the output easier to read: 

```python
print(target_host, target_port, status, sep=" | ") # 192.168.1.10 | 443 | OPEN
```

Or you could use a colon symbol: 

```python
print(target_host, target_port, status, sep=":") # 192.168.1.10:443:OPEN
```

You can even tell Python to use **nothing at all** as the separator:

```python
print(target_host, target_port, status, sep="") # 192.168.1.10443OPEN
```

The key thing to remember here is that `sep` controls what `print()` puts between the arguments you give to it.

### Managing Line Breaks with `end`

As mentioned earlier, `print()` automatically adds a newline character `\n` at the end of its output.

This means that each call to `print()`, Python displays your message and then moves the cursor to the beginning of the next line:  

```python
print("Scanning target...") 
print("Host: 192.168.1.10") 
print("Status: UP") 
```

Output: 

```
Scanning target... 
Host: 192.168.1.10 
Status: UP
```

Behind the scenes, Python is essentially doing this:

```python
print("Scanning target...", end="\n")
print("Host: 192.168.1.10", end="\n") 
print("Status: UP", end="\n") 
```

The `end` keyword argument controls **what Python adds at the end of the output**. By default, the value is `"\n"`.

However, as mentioned earlier, you can change this behavior. For example, let's say you want multiple `print()` calls to remain on the same line: 

```python
print("[*] Scanning", end=" ") 
print("192.168.1.10", end=" ") 
print("...") 
```

Output: 

```
[*] Scanning 192.168.1.10 ...
```

Of course, you can also use other characters or strings:

```python
print("[+] Host", end=" | ") 
print("192.168.1.10", end=" | ") 
print("UP") 
```

Output: 

```
[+] Host | 192.168.1.10 | UP
```

Or, if you don't want anything added at the end, you can simply use an empty string:

```python
print("Port", end="") 
print("443") 
```

Output:

```
Port443
```

The `sep` and `end` arguments might not seem like a big deal at first, but they can give you a powerful control over how your programs communicate and display information.

### Repeating Characters with `*`

If you've ever used a cybersecurity tool from the cli, certainly you've seen those beautiful visual separators that make the output pleasant to read. 

The technique behind them is actually pretty simple: **repeat the same character or string multiple times**.

Python lets you do this by using the `*` operator with a string. For instance: 

```python
print("=" * 40)
```

Output: 

```
========================================
```

Here, Python is taking the string `=` and repeating it 40 times. 

This makes it easy to create visual separators for your tools, like so:

```python
print("=" * 40) 
print("            PORT SCAN RESULTS") 
print("=" * 40)
```

Output:

```
========================================
            PORT SCAN RESULTS
========================================
```

Beautiful! 😎

Of course you can use the same technique with other characters: 

```python
print("-" * 30)
print("#" * 30)
print("*" * 30)
```

Output: 

```
------------------------------
##############################
******************************
```

This also may seem like a small feature, but you'll find it surprisingly useful when building command-line security tools where **clear, readable output matters**.

## Challenge Time - Build an Interactive Network Recon Profile

Alright, you already created a basic network reconnaissance profile. Now it's time to make it a little more interactive.

This time, instead of hardcoding the target information directly into your script, your tool should now ask the user for the information it needs.

Your program should ask the user for:

- **Hostname**
- **IP address**
- **Operating system**
- **Primary service**
- **Port**
- **Connection encrypted** (`True` or `False`)

For example:

```
Enter hostname: web-server-01 
Enter IP address: 192.168.56.101 
Enter operating system: Linux 
Enter primary service: HTTPS 
Enter port: 443 
Is the connection encrypted? True
```

And after collecting the information, your tool should display a readable summary similar to:

```
[*] Collecting target information... 
[+] Target profile created 

============================================= 
            NETWORK RECON PROFILE 
=============================================

Hostname: web-server-01 
IP Address: 192.168.56.101 
Operating System: Linux 
Primary Service: HTTPS 
Port: 443 
Encrypted: True 

=============================================
```

Your solution should: 

1. Use `input()` to collect information from the user.
2. Store each piece of information in a meaningful variable.
3. Convert the **port** from a string to an integer using `int()`.
4. Use `print()` in whatever way you think makes sense to display the information.
5. Use repeated characters such as `=`, `-`, or `*` to create the console banner with its visual separators. 

Don't worry if your solution doesn't look exactly like someone else's. There are often multiple valid ways to represent the same information in Python. What matters is that you understand what your code is doing and why it works.

Keep learning, keep building, and stay tuned. 🤖
