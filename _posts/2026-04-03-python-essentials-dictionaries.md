---
title: Python Essentials - Dictionaries
image:
  path: /assets/2026-04-03-python-essentials-dictionaries/7sak97sak97sak97.png
date: 2026-04-03 05:00:00 -0600
categories: [Programming Languages, Python]
tags: [python, python-fundamentals]     # TAG names should always be lowercase
---

A **dictionary** is an **unordered**, **mutable** data structure that allows you to store pieces of information as **key/value pairs**. For example: 

```python
psyker = {
	'damage': 3, 
	'warp_charge_cost': 5.5, 
	'factions': ['Space Marines', 'Imperial Guard', 'Chaos Marines'],
	'aligned_with_imperium': True
}
```

Think of it like a real dictionary: you look up a _word_ (the key) to get its _definition_ (the value). 

## Accessing Values

The way you start retrieving values from a dictionary is by specifying its corresponding **key**, like so:  

```python
psyker_damage = psyker['damage']
psyker_factions = psyker['factions']

print(psyker_damage)   # 3
print(psyker_factions) # ["Space Marines", "Imperial Guard", "Chaos Marines"]
```

If you try to access a key that doesn't exists in the dictionary, Python will bite you back: 

```python
psyker_weapon = psyker['primary_weapon']
# KeyError: 'primary_weapon'
```

One way to prevent this is to check if the key exists beforehand using **Membership Testing**: 

```python
'primary_weapon' in psyker
# False
```

Another common mistake is trying to access a value by its position as if it were a list. That just doesn't make any sense, and it won't work anyway: 

```python
psyker_damage = psyker[0]
# KeyError: 0
```

Just remember: dictionaries are about _keys_, not _positions_.

## Nested Dictionaries

When working with dictionaries you have the ability to nest a dictionary inside another dictionary. For example: 

```python
factions = {
	'space_marines': {
		'unit_count': 1200,
		'primary_weapons': ['Power Fist', 'Boltgun'],
		'aligned_with_imperium': True,
	},
	'orks': {
		'unit_count': 3000,
		'primary_weapons': ['Shoota', 'Choppa'],
		'aligned_with_imperium': False,
	}
}
```

Now to access your favorite faction's primary arsenal, you need to chain the keys, like this: 

```python
space_marines_arsenal = factions['space_marines']['primary_weapons']
orks_arsenal = factions['orks']['primary_weapons']

print(space_marines_arsenal)   # ["Power Fist", "Boltgun"]
print(orks_arsenal)            # ["Shoota", "Choppa"]
```

## Dictionaries on the fly

Sometimes you don’t know in advance what keys and values your dictionary will contain, and that’s perfectly fine. You can build a dictionary **on the fly** as your program runs:

```python
# Start with an empty dictionary
imperial_guard = {}

# Then, add key/value pairs as needed
imperial_guard['primary_weapon'] = "Lasgun"
imperial_guard['secondary_weapon'] = "Bayonet"
imperial_guard['armor_type'] = "Flak Armor"
imperial_guard['bravery_skill'] = 7
imperial_guard['unit_size'] = 10
imperial_guard['allied_with_imperium'] = True
```

Each assignment creates a new entry in the dictionary or updates it if the key already exists. 

Now you can access this dictionary's values the same way as any other dictionary: 

```python
print(imperial_guard['primary_weapon']) # "Lasgun"
```

## Dictionary Keys

One powerful feature of dictionaries is that keys aren’t limited to just strings, you can use **many different data types** as keys. For example: 

```python
emperor = {
	1: {'title': 'The Emperor of Humankind' },
	'home_world': 'Old Terra',
	10**9: 'Age (years)',
	False: 'Worshiped as God by just few',
	20.0: 'Power level (out of 10)',
	(31, 'M30'): 'The Great Crusade begins' 
}

print(emperor[1])            # {'title': 'The Emperor of Humankind' }
print(emperor['home_world']) # 'Old Terra'
print(emperor[10**9])        # 'Age (years)'
print(emperor[False])        # 'Worshiped as God by just few'
print(emperor[20.0])         # 'Power level (out of 10)'
print(emperor[(31, 'M30')])  # 'The Great Crusade begins'
```

However, the most important rule here is: **keys must be immutable**. 

If you closely look at the keys, you'll notice that they're all of immutable type. This is a restriction that your keys must stick to. 

If you try to use a mutable data structure as a key, for example a list, Python will complain: 

```python
user = {
	'name': 'Alice',
	'age': 30,
	[2024, 8, 10]: 'Last Login date'
}
# TypeError: unhashable type: 'list'
```

## Common Operations on Dictionaries

Dictionaries have a well-defined API to manipulate the information within. 

### Safely getting a value

A more flexible way to retrieve a value from a dictionary is by using the `.get()` method. This lets you **access a key without raising an error** if the key doesn't exist. 

If the key exists, `.get()` returns the value associated, otherwise returns `None`: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_name = user.get('name')
user_birthday = user.get('birthday')

print(user_name)     # 'John Doe'
print(user_birthday) # None
```

Alternatively, if the key is not found you can fallback to any other value of your preference instead of just `None`: 

```python
# birthday key doesn't exist in user dictionary.
user_birthday = user.get('birthday', '1990-04-15')

print(user_birthday) # '1990-04-15'
```

This way, your program keeps running smoothly without needing extra checks.

### Getting all key/value pairs

You can retrieve all the data in a dictionary as **key–value pairs** using the `.items()` method: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_items = list(user.items())

print(user_items) 
# [('name', 'John Doe'),('age', 34),('email_verified', True)]
```

As shown, the `.items()` method returns a sequence of tuples, where each tuple contains a key and its corresponding value. This is specially useful when you want to iterate over both keys and values at the same time.

### Getting only the keys

In this scenario, your only interest is getting the dictionary's keys: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_keys = list(user.keys())

print(user_keys)
# ['name', 'age', 'email_verified']
```

For that, you use the `.keys()` method. 

### Getting only the values

In this case, your only interest is getting the dictionary's values: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_values = list(user.values())

print(user_values) 
# ['John Doe', 34, True]
```

For that, you use the `.values()` method. 

### Removing a key

The simplest way to remove a key (and its associated value) is by using the `del` keyword:

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

# Remove the email_verified key along with its associated value
del user['email_verified']

print(user)
# {'name': 'John Doe', 'age': 34}
```

#### Using `pop()` to remove and retrieve a value

Another common approach is the `.pop()` method. It removes the key and **returns its value at the same time**:

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_email_verified = user.pop('email_verified')

print(user)
# {'name': 'John Doe', 'age': 34}

print(user_email_verified)
# True
```

As you can see, the associated value of the removed key is returned. Just keep in mind that if the key doesn't exist in the dictionary, `.pop()` will raise an error. 

To prevent Python from raising an exception, you can pass a **default value** to `.pop()`. If the key doesn't exist, that value will be returned instead: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_likes = user.pop('likes', 0)

print(user)
# {'name': 'John Doe', 'age': 34, 'email_verified': True}

print(user_likes)
# 0
```

This way, your code stays safe and predictable even when keys might be missing.

### Removing a key/value pair

If your intention is to remove a key/value pair from a dictionary, you can use the `.popitem()` method. You don't need to provide any argument to this method, `.popitem()` will remove the last key/value pair added to the dictionary: 

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user_email_verified = user.popitem()
user_age = user.popitem()

print(user)
# {'name': 'John Doe'}

print(user_email_verified) 
# ('email_verified', True)

print(user_age)
# ('age', 34)
```

As you can see, each removed entry is returned as a **tuple** in the form `(key, value)`.

Nonetheless, if you call `.popitem()` on an empty dictionary, Python will raise an error:

```python
user = {}
user.popitem()
# KeyError: 'popitem(): dictionary is empty'
```

### Clearing a dictionary

If you want to remove **all** key/value pairs at once, you can use the `.clear()` method:

```python
user = {
	'name': 'John Doe',
	'age': 34,
	'email_verified': True
}

user.clear()

print(user) # {}
```

After calling `.clear()`, the dictionary is simply empty.

## Merging Dictionaries

You can combine the contents of one dictionary into another using the `.update()` method. However, before looking at merging, it helps to know that `.update()` can also be used to **add new key/value pairs** to your dictionary. For example: 

```python
facebook_profile = {  
	"username": "Buttman",  
	"followers": 800,  
	"likes": 2500  
}  
  
facebook_profile.update({"posts": 300, "views": 12000})  
  
print(facebook_profile)  
# {'username': 'Buttman', 'followers': 800, 'likes': 2500, 'posts': 300, 'views': 12000}
```

Now let’s use `.update()` to merge two dictionaries:

```python
facebook_profile = {	
	"username": "Buttman", 
	"followers": 800, 
	"likes": 2500
}

twitter_profile = {
	"username": "Mr. Buttman", 
	"followers": 1500, 
	"posts": 300 
}

facebook_profile.update(twitter_profile)

print(facebook_profile)
# {'username': 'Mr. Buttman', 'followers': 1500, 'likes': 2500, 'posts': 300}
```

After the merge, `facebook_profile` is updated with data coming from `twitter_profile`: 

* If a key doesn't exist, it gets added. In the case of `facebook_profile` the `post` structure doesn't exist, therefore it gets added. 
* If a key already exists, its value is **overwritten**. In the case of `facebook_profile`, both `username` and `followers` already exist, therefore they get overwritten by the values from `twitter_profile`. 

That’s it for the basics of dictionaries. Great job making it this far, see you in the next lesson! 💡
