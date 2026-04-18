---
title: Python Essentials - Tuples
image:
  path: /assets/2026-04-01-python-essentials-tuples/6r52ve6r52ve6r52.png
date: 2026-04-01 05:00:00 -0600
categories: [Programming Languages, Python]
tags: [python, python-fundamentals]     # TAG names should always be lowercase
---

A **tuple** is the **immutable** sibling of a list. Just as you would do with a list, you can store multiple pieces of information of different types in a tuple:  

```python
weapons = ('Boltgun', 24.0, True, 'Lascannon', 48.0, False)
```

Did you notice the use of `()` instead of `[]` when defining a tuple?.


**Immutability** is the key differentiator between a *list* and a *tuple*. This simply means that, once created, a tuple's size cannot be changed by adding new elements to or removing existing ones from it.  Let's demonstrate this by attempting to remove some elements from the `weapons` tuple by using the `del statement` and the well-known **data slicing technique**: 

```python
# Try to remove an element using del
del weapons[3]
# TypeError: 'tuple' object doesn't support item deletion

# Try to remove an element using data slicing
weapons[3:4]
# TypeError: 'tuple' object does not support item assignment
```

Both approaches failed miserably 😖, and don't even bother using any of the common list methods to manipulate the items in a tuple, they're practically inexistent:

```python
weapons.append
# AttributeError: 'tuple' object has no attribute 'append'

weapons.remove
# AttributeError: 'tuple' object has no attribute 'remove'
```

On top of that, once all the elements have been allocated in a tuple, **they cannot be modified**. For instance, if you try to update an individual item in a tuple: 

```python
weapons[3] = "baby rattle"
# TypeError: 'tuple' object does not support item assignment
```

You'll get an error. This is because tuples don't support direct assignment once all elements have been allocated. 

When working with tuples just keep in mind that they are **immutable data structures**. 

Alright, this might feel discouraging for you, but you can still have fun by *accessing their items by index*, getting the portions you need with *data slicing*, and checking the existence of elements with *membership Testing*. If any of this sounds unfamiliar to you, you can check out my article on [Python Lists](https://jjeuribe.github.io/posts/python-essentials-lists/) where I explore these topics on more depth.

## Methods Supported

Tuples have only two methods: `count()` and `index()`.

By using the `count()` method can you check how many times a given item is present in the tuple: 

```python
favorite_colors = ('blue', 'green', 'red', 'blue', 'green', 'red', 'blue', 'orange')

favorite_colors.count('blue') # 3 
```

On the other hand, the `index()` method helps you locate the first matching item in a tuple and retrieve the index associated to it: 

```python
favorite_colors = ('blue', 'green', 'red', 'blue', 'green', 'red', 'blue', 'orange')

# If the item exists, return index position. 
favorite_colors.index('red') # 2

# Otherwise, raises a ValueError: tuple.index(x): x not in tuple
favorite_colors.index('pink')
```

Alright, I know tuples have a limited set of features, but you can still rely on some Python’s built-in functions to work with them effectively. Let's take a look.

### Getting the length of a tuple

```python
lucky_numbers = (7, 14, 21, 28, 35, 42)

len(lucky_numbers) # 6
```

### Getting the largest element from a tuple

```python
lucky_numbers = (7, 14, 21, 28, 35, 42)

max(lucky_numbers) # 42
```

### Getting the smallest element from a tuple 

```python
lucky_numbers = (7, 14, 21, 28, 35, 42)

min(lucky_numbers) # 7
```

### Converting a list into a tuple 

```python
lucky_numbers = [7, 14, 21, 28, 35, 42]

tuple(lucky_numbers) # (7, 14, 21, 28, 35, 42)
```

In this case, you get a new data structure as a tuple, the `lucky_numbers` data source remains unaffected.

## Then, what are tuples good for?

At this point you might be thinking that tuples don't seem very useful, so the big question is: * what are they good for anyway?*. Here are some good reasons: 

1. Since they allocate just a fixed amount of memory, they have less memory overhead when compared to lists. 
2. Because of their immutable nature, you ensure data integrity which guarantees data stability. Data source always remains unchanged.
3. They tend to be more efficient than lists in terms of creation and iteration. This is important when working with large datasets. 
4. They're commonly used for storing heterogenous data that doesn't or barely change, whereas, lists are typically use for storing homogeneous data that frequently changes.
5. As we'll see later on, they can be used as keys for dictionaries.
