---
title: Homework Solutions 2
author: Guess Who
date: 2020-12-12 15:36:00 +0300
categories: [Python, Lectures, Solutions]
tags: [solutions]
math: false
---

# 1

We first need to figure out how to add letter from the start of a string to the end. For our first case let's try taking the first letter of `'string'` and sticking it to the end.

Slice the string and take the first letter:

```python
>>> 'string'[:1]
's'
```

Slice the string and take everything except the first letter:

```python
>>> 'string'[1:]
'tring'
```

Snap the first letter to the end of everything-that-is-not-the-first-letter:

```python
>>> 'string'[1:]+'string'[:1]
```

Now that we know how to rotate strings, we can start building our function.

## How?

As you know if we rotate a string enough times (`len(string)` times to be exact) we'll get our initial string back 

| 0th  | 1st  | 2nd  | 3rd  | 4th  |
| ---- | ---- | ---- | ---- | ---- |
| moot | ootm | otmo | tmoo | moot |

So we can write a for loop that will rotate one of our strings this many times, and check if any of the rotations are equal to the other string.

## Doing it

Defining our function

```python
def rotation_test(str1, str2):
```

To write the loop, we'll use the `range()` function. Since we can at most rotate our string `len(string)` times, that's what we'll give to `range()`. Using `range()` will also allow us to check for the initial condition since it starts from 0 (`'string'[0:]+'string'[:0] == 'string'`)  And arbitrarily I've chosen the second string (`str2`) to rotate.

```python
    for i in range(len(str2)):
```

Now we'll rotate `str2` and check whether it's the same as `str1` at every rotation. If they are the same, we'll return true.

```python
        if str2[i:]+str2[:i] == str1:
            return True
```

So if they are the same somewhere inside the loop, the function will return `True` and the interpreter won't read the rest of the function but if it isn't, the function will return `None`. We can fix this by returning `False` after the loop (again, this will only be read if the strings aren't the rotations of each other).

```python
    return False
```

## The problem

Let's write some tests.

```python
print(rotation_test("nicole", "icolen"))
print(rotation_test("nicole", "lenico"))
print(rotation_test("aabaaaaabaab", "aabaabaabaaa"))
print(rotation_test("booyah", "yahbah"))
print(rotation_test("", ""))
```

Now clearly, the first three should give us `True`, the fourth `False` and the last one `True`. But what happens?

```
True
True
True
False
False
```

This is because no matter how you slice `''`, you get `''`. We can fix this by just adding an if statement to check whether both of our strings is `''` and return `True` if so. This is to be added before the final `return`.

```python
    if str1+str2 == '':
        return True
```

 Here we shortened our code by using this statement since `str1+str2` is only `''` when both `str1` and `str2` is `''`. (Alternate way to do it would be `str1 == '' and str2 == ''`)

## Code in full

```python
def rotation_test(str1, str2):	
    if str1+str2 == '':
		return True
	for i in range(len(str2)):
		if str1 == str2[i:]+str2[:i]:
			return True
	return False
```

## Smarter way

Like always, there is a smarter way. Think about the rotations `'string'` and `'trings'`. What happens when we double the second string? We get `'tringstrings'`. Notice anything peculiar? Yes. Inside the doubled version of our second string, sits our first string. So we can just make our function

```python
def rotation_test(str1, str2):
    double = str2+str2
    if str1 in double:
        return True
    else:
        return False
```

### The problem

The problem is, when we do `rotation_test('string', 'stringas')`, we'll get `True` but we don't want this, since our strings have to be the same length. We can just add a statement that returns  `False` if these strings aren't the same length.

```python
    if len(str1) != len(str2):
        return False
```

Although this is still a dumb solution because first, we check if `str1 in double` and return `True` if it's `True` and `False` if it's `False`. Do you see the redundancy? Why don't we just return `str1 in double`?

 ```python
    return str2 in double
 ```

### Code in full

```python
def rotation_test(str1, str2):
    if len(str1) != len(str2):
        return False
    double = str2+str2
    return str2 in double
```

### Golfed

```python
def rotation_test(i, j):
  return (len(i) == len(j)) and (j in (i + j))
```

# 2

Not much to do in this question. We'll just make a dictionary that has numbers as keys and add matching stuff to our answer variable. We'll build on the advanced solution as you've learned about splits.

```python
def time2words(string):
    hour, minute = int(string.split(':')[0]), int(string.split(':')[1])
```

Defining our dictionary

```python
    dictionary = {0:'', 1:'one', 2:'two', 3:'three', 4:'four', 5:'five', 6:'six', 7:'seven', 8:'eight', 9:'nine', 10:'ten', 11:'eleven', 12:'twelve', 13:'thirteen', 14:'fourteen', 15:'fifteen', 16:'sixteen', 17:'seventeen', 18:'eighteen', 19:'nineteen', 20:'twenty', 30:'thirty', 40:'forty', 50:'fifty'}
```

This has all the numbers we need. Let's try to find a way to convert `hour` and `minute` using this dictionary. 

## Hour part

Let's get our appendix similarly to how we did last time

```python
    if 12 >= hour > 24:
        appendix = 'pm'
    else:
        appendix = 'am'
```

And let's translate our 24-hour system to 12-hour like the last time using mod

```python
    hour = hour%12
```

 Now there are a few cases we need to consider

1. `hour` is 0 in which case we'll be assigning `hour_writing = 'twelve'` manually (We do this because we defined `0:''` in our dictionary)
2. `hour` is anything else in which case we'll just look it up on the dictionary

```python
    if hour == 0:
        hour_writing = 'twelve'
    else:
        hour_writing = dictionary[hour]
```

And the hour part is done.

## Minute part

We'll split our `minute` to 10s and 1s like we did last time.

```python
    minute_ones = minute%10
    minute_tens = minute-minute_ones
```

Now we can get the correspondents of these from our dictionary

```python
    minute_writing = dictionary[minute_tens]+' '+dictionary[minute_ones]
```

And we'll fix it if our `minute` is something like 12 or 15

```python
    if 10 < minute < 20:
        minute_writing = dictionary[minute]
```

## Putting it all together

We just need to return what we have and fix the spaces.

```python
    if (minute_tens == 0) and (minute_ones == 0):
        return hour_writing+' '+appendix
    elif (minute_tens != 0) and (minute_ones == 0):
        return hour_writing+' '+minute_writing+appendix
    elif (minute_tens == 0) and (minute_ones != 0):
        return hour_writing+minute_writing+' '+appendix
    else:
        return hour_writing+' '+minute_writing+' '+appendix
```

## Code in full

```python
def time2words(string):
    hour, minute = int(string.split(':')[0]), int(string.split(':')[1])
    dictionary = {0:'', 1:'one', 2:'two', 3:'three', 4:'four', 5:'five', 6:'six', 7:'seven', 8:'eight', 9:'nine', 10:'ten', 11:'eleven', 12:'twelve', 13:'thirteen', 14:'fourteen', 15:'fifteen', 16:'sixteen', 17:'seventeen', 18:'eighteen', 19:'nineteen', 20:'twenty', 30:'thirty', 40:'forty', 50:'fifty'}
    
    if 12 <= hour < 24:
        appendix = 'pm'
    else:
        appendix = 'am'
        
    hour = hour%12
    if hour == 0:
        hour_writing = 'twelve'
    else:
        hour_writing = dictionary[hour]
        
    minute_ones = minute%10
    minute_tens = minute-minute_ones
    minute_writing = dictionary[minute_tens]+' '+dictionary[minute_ones]
    if 10 < minute < 20:
        minute_writing = dictionary[minute]
    if (minute_tens == 0) and (minute_ones == 0):
        return hour_writing+' '+appendix
    elif (minute_tens != 0) and (minute_ones == 0):
        return hour_writing+' '+minute_writing+appendix
    elif (minute_tens == 0) and (minute_ones != 0):
        return hour_writing+minute_writing+' '+appendix
    else:
        return hour_writing+' '+minute_writing+' '+appendix
```

## Golfing

Golfing is writing code with as little characters as possible.

```python
def t(s):
    d = {0:'', 1:'one', 2:'two', 3:'three', 4:'four', 5:'five', 6:'six', 7:'seven', 8:'eight', 9:'nine', 10:'ten', 11:'eleven', 12:'twelve', 13:'thirteen', 14:'fourteen', 15:'fifteen', 16:'sixteen', 17:'seventeen', 18:'eighteen', 19:'nineteen', 20:'twenty', 30:'thirty', 40:'forty', 50:'fifty'}
    h, m = map(int, s.split(':'))
    return ' '.join(f"{'twelve' if h%12 == 0 else d[h%12]} {d[m] if 10 < m < 20 else d[m-m%10]+' '+d[m%10]} {'pm' if 12 <= h < 24 else 'am'}".split())
```

# 3

I've modified the question a bit to be more useful. Of course you can do it with pre-defined values.

```python
def table(function, values):
    print('x    |  f(x)\n-----------')
    for i in values:
        print(str(i)+'    |    '+str(function(i)))
```

This is pretty basic. The first print is to set up table titles. Then we loop over each value in `values` and print value-output pairs. To have a nicer table I've also added a separation in between these. Of course, to be able to add it we needed to convert our `i` and `function(i)` to `str` (They could have been integers depending on the function and we certainly don't want that). For a little test let's make a simple function

```python
def func(x):
	return x**2
```

And try printing a table of our function

```python
table(func, [0,1,2,3,4])
```

Output:

```
x    |  f(x)
-----------
0    |    0
1    |    1
2    |    4
3    |    9
4    |    16
```

# 4

We'll just take a string, and use a split.

```python
def reverse_string(string):
    return string[::-1]
```

Our split starts at the beginning of the string (`0`) and ends at the end (`len(string)`) but it counts backwards.