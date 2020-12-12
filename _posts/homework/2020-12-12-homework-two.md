---
title: Homework 1
author: Guess Who
date: 2020-12-12 14:36:00 +0300
categories: [Python, Lectures, Homework]
tags: [homework]
math: false
---

# 1

Notice anything peculiar about "ikbal" and "kbali?" When we take the first letter of ikbal and snap it to the end, we get kbali. We can continue this and get balik, alikb, likba and finally return to ikbal. Let's call such words rotations of each other. Write a function that takes two strings and checks whether these strings are rotations of each other. For example

```python
>>> def rotation_test(str1, str2):
    ...
    ...
    ...
>>> rotation_test('ikbal', 'kbali')
True
>>> rotation_test('xxx', 'xxx')
True
>>> rotation_test('xxy', 'xyx')
True
>>> rotation_test('booyah', 'yahboo')
True
>>> rotation_test('hello', 'hlleo')
False
>>> rotation_test('ikbal', 'ilkba')
False
```

# 2

Write the last week's function but use dictionaries this time.

# 3

Make a function that will take a function and return a table of values.

# 4

Make a function that will return the reversed version of any string. For example:

```python
>>> def reverse_string(string):
    ...
    ...
>>> reverse_string('ikbal')
labki
>>> reverse_string('xxy')
yxx
```

