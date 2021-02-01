---
title: Lecture Notes 3
author: Guess Who
date: 2020-12-19 13:36:00 +0300
categories: [Python, Lectures, Notes]
tags: [lecture]
math: true
---

# Input

Here we'll make use of the `input()` function (*gasps*).

## Example 1

```python
>>> input()
boo
'boo'
```

Here you might notice that `input()` acted like the `echo` command in bash. To be honest, this is all there is to it. When you use `input()`,  the interpreter will wait for you to enter an input and then store that input in temporary memory. 

## Example 2

In a program we can capture this input and store it in a variable

```python
our_input = input()
print(our_input)
```

Like I've mentioned before, you can't run this in Sublime, so we'll launch a terminal and run it there.

```bash
$ python3 input.py
hello
hello
```

Sure enough, it works. And that's all there is to it. The last thing to keep in mind can be, no matter what you enter, `input()` will give you a string (So the users can't input arbitrary commands and take over your program). So if you want to work with integers you need to first convert your input to an integer.

```python
>>> type(input())
123
<class 'str'>
```

```python
>>> type(int(input()))
123
<class 'int'>
```

# Default arguments

This section is very simple. It is basically about giving function arguments a default value. (remember how `print()` had a default argument `\n`?)

## Example 1

Let's make a function that will invert a boolean it takes. For example if we give it `True` it'll give us `False`. And let's make it use `True` by default.

```python
def invert(arg = True):
    return not arg
```

and that's all there is to it! If you want a default for an argument, just do `arg = <your desired default>` where you define your arguments.

# Scoping

The point of scoping is, to each his own. What do I mean by that? I mean that if you define a variable inside a function, that variable is _local_ not _global_ meaning, it is only defined inside that function.

## Example 1

Let's make a function and define a variable inside it. Afterwards, we'll try printing our variable. 

```python
def definevar():
    x = 4
    print("Value of x inside the fn", x)
definevar()
print("Value of x outside the fn", x)
```

Output:

```
Value of x inside the fn 4
Traceback (most recent call last):
  File "/home/user/lessons/scoping.py", line 5, in <module>
    print("Value of x outside the fn", x)
NameError: name 'x' is not defined
```

As you see, the print statement inside our function ran just fine, but the one on the outside didn't because `x` was only defined inside the function.

## Example 2 

Let's try analyzing the code below

```python
def f(x):
	def g():
		x = 'abc'
		print("x =", x)
	def h():
		z = x
		print("z =", z)
	x = x+1
	print("x =", x)
	h()
	g()
	print("x =", x)
	return g

x = 3

z = f(x)

print("x =", x)
print("z =", z)
z()
```

Output:

```
x = 4
z = 4
x = abc
x = 4
x = 3
z = <function f.<locals>.g at 0x7f34648cdbf8>
x = abc
```

Oh wow. Now what happened here? Let's trace it back

1. line 15 `x = 3` 

2. line 17 `z = f(x)` 

   1. line 8 `x = x+1`

   2. line 9 `print("x =", x)`

   3. line 10 `h()`

      1. line 6 `z = x`

      2. line 7 `print("z =", z)`

         This will print a local variable and not the one in 2.

   4. line 11 `g()`

      1. line 3 `x = 'abc'`
      2. line 4 `print("x =", x)`

   5. line 12 `print("x =", x)`

   6. line 13 `return g`

      Notice how we return the `g` function itself. So `f()`'s value is a function itself. This means when we do `z = f()` then `z == g`. This is why we got `z = <function f.<locals>.g at 0x7f34648cdbf8>` in our output.

3. line 19 `print("x =", x)`

4. line 20 `print("z =", z)`

5. line 21 `z()`

   Since `z == g` this is basically `g()`

But how do we define a global variable inside a function? Is all hope lost? Simple:

```python
def f():
    global x
    x = 5
f()
print(x)
```

Output:

```
5
```

`global`allows us to make our variables global. (shocking!)

# Recursion

Recursion is calling a function inside that function. What do I mean?

## Example 1

Consider the Fibonacci function

```python
def fib(n):
    a,b = 0,1
    for i in range(n):
        a,b = b, a+b
    return a
```

To understand this function better, let's take a look at the values `a` and `b` have `for i in range(n)`

| Step | a    | b    |
| ---- | ---- | ---- |
| 0    | 0    | 1    |
| 1    | 1    | 1    |
| 2    | 1    | 2    |
| 3    | 2    | 3    |
| 4    | 3    | 5    |
| 5    | 5    | 8    |
| 6    | 8    | 13   |
| 7    | 13   | 21   |
| 8    | 21   | 35   |

So we're just slowly shifting our pairs through the Fibonacci set here. I like to think of it like we're sliding a magnifier through the Fibonacci set.

![](/assets/img/posts/python/3/slider.png)

How might we write this with recursion? Simple. Actually it's simpler than the one without recursion. Let's remember what the Fibonacci function actually meant. 

\\( F(N) = F(N-1)+F(N-2) \\) where \\( F(0)=1 \\) and \\( F(1)=1 \\)

This seems like the exact recipe for a recursive function, doesn't it? Translating math into pseudo-code (code that doesn't run) we'd get:

` fib(n) returns fib(n-1) + fib(n-2)` where `fib(0) returns 1` and `fib(1) returns 1`

So first we'll check if `n` is `0` or `1` and return `1` if that's the case. If not, we'll return `fib(n-1)+fib(n-2)`.

```python
def fib(n):
    if n == 0 or n == 1:
        return 1
    else:
        return fib(n-1)+fib(n-2)
```

Done.

## Example 2

A palindrome is a phrase whose characters are read the same from the left or the right. For example: "Madam, I'm Adam."

In this example we'll try to make a function that tests whether a string is a palindrome. This is also a nice opportunity for me to introduce you to the **divide and conquer paradigm**. Divide and conquer as it's name is about breaking down a problem into smaller problems that are easier to handle. We'll make a parent `isPalindrome(s)` function with child functions, and each child function will handle a different sub-problem of our goal. Our sub-problems are:

1. Remove all spaces and punctuation from the string and turn it into a single word with all lowercase characters.
2. Recursively check whether the last and the first character in our word is the same.

For the "Madam, I'm Adam." example,

1. Turn our string into "madamimadam"

2. 1. Check whether the first and last letters are the same

   2. Check whether the second and second from last letters are the same.

      ...

We'll first create our function and inside it, create a `toChars(s)` function.

```python
def isPalindrome(s):
    def toChars(s):
```

First we'll redefine our string to be all lowercase.

```python
        s = s.lower()
```

To remove punctuation we'll first make an empty string. We'll go over our string character by character, and then, we'll go and check if the character is a letter or a number. If that's the case we'll add our character to our empty string.

```python
    letters = ''
    for c in s:
        if c in 'abcdefghijklmnopqrstuvwxyz':
            letters = letters + c
```

Finally, we can return our now sanitized string.

```python
        return letters
```

Our `toChars(s)` function is complete. Now, on to the `isPal(s)` function. This is where the magic will happen.

```python
    def isPal(s):
```



Now as you might've guessed we need an initial condition. To build recursive functions we need to predefine _some_ situations so that the function has something to fall back on. In this case our initial condition is when there are no letters or only one letter left. 0 and 1 letter words are always palindromes. 

```python
    if len(s) <= 1:
        return True
```

Now if the initial condition isn't met we'll recursively check palindrome-ness. How might we do this? We'll check the first and last letters; remove them from our string; and re-run our function with the shorter, new string.

```python
    else:
        answer = s[0] == s[-1] and isPal(s[1:-1])
        return answer
```

Here we have a logical statement. The interpreter will check the first statement, `s[0] == s[-1]` _and_ then run `isPal(s[1:-1])` which will check `s[1:-1][0] == s[1:-1][1] ` and so on until `len(s)` is smaller than or equal to 1.

Good. Now we need to put this all together and run our functions and return the result. Think of this as composite functions.

```python
    return isPal(toChars(s))
```

And this concludes our function.

Using this example I want to introduce yet another concept: **test suites**. Test suites are functions that test other functions.

```python
def testIsPalindrome():
```

We'll keep it simple and test only two strings: dogGod (_is_ a palindrome) and doGood (_isn't_ a palindrome). We'll just print what we're testing and a logical statement stating whether the test went fine.

```python
def testIsPalindrome():
    print("Try dogGod")
    print(isPalindrome("dogGod") == True)
    print("Try doGood")
    print(isPalindrome("doGood") == False)
```

And finally we'll run the tests:

```python
testIsPalindrome()
```

The output is:

```
Try dogGod
True
Try doGood
True
```

So we're all good!

### Code in full 

```python
def isPalindrome(s):
    def toChars(s):
        s = s.lower()
        letters = ''
        for c in s:
            if c in 'abcdefghijklmnopqrstuvwxyz':
                letters = letters + c
        return letters
    def isPal(s):
        if len(s) <= 1:
            return True
        else:
            answer = s[0] == s[-1] and isPal(s[1:-1])
            return answer

    return isPal(toChars(s))

def testIsPalindrome():
    print("Try dogGod")
    print(isPalindrome("dogGod"))
    print("Try doGood")
    print(isPalindrome("doGood"))

testIsPalindrome()
```

# Modules

```python
>>> import math
>>> math.pi
3.141592653589793
>>> math.e
2.718281828459045
>>> math.sqrt(2)
1.4142135623730951
```

```python
>>> from math import *
>>> pi
3.141592653589793
>>> sqrt(2)
1.4142135623730951
>>> e
2.718281828459045
```

# List methods

Speaking of tying loose ends, we need to come back to lists (_again, I know, but for the last time I promise_)

```python
>>> lis = [0, 1, 2, 3, 4]
```

1. **Inserting:** Adding an item to a list at a specific location.

```python
>>> lis.insert(1, 'inserted')
>>> lis
[0, 'inserted', 1, 2, 3, 4]
```

2. **Count:** How many times an object appears in a list.

```python
>>> lis.count(1)
1
```

3. **Append:** Adds an item to the end of the list.

```python
>>> lis.append('appended')
>>> lis
[0, 'inserted', 1, 2, 3, 4, 'appended']
```

4. **Extend:** Adds a list to the end of another list.

```python
>>> lis2 = ['extended1', 'extended2', 'extended3']
>>> lis.extend(lis2)
>>> lis
[0, 'inserted', 1, 2, 3, 4, 'appended', 'extended1', 'extended2', 'extended3']
```

5. **Pop:** Removes an element at an index, and also returns that element. ()

```python
>>> lis.pop(0)
0
>>> lis
['inserted', 1, 2, 3, 4, 'appended', 'extended1', 'extended2']
```

This will remove the last element if we use it without an attribute.

```python
>>> lis.pop()
'extended3'
>>> lis
[0, 'inserted', 1, 2, 3, 4, 'appended', 'extended1', 'extended2']
```

6. **Reverse:** Reverses a list.

```python
>>> lis.reverse()
>>> lis
['extended2', 'extended1', 'appended', 4, 3, 2, 1, 'inserted']
```

7. **Sorting:** Sorts a list.

The default sort for a list is ascending. 

```python
>>> numlist = [5, 6, 8, 10, 22, 9, 3, 1, 2]
>>> numlist.sort()
>>> numlist
[1, 2, 3, 5, 6, 8, 9, 10, 22]
```

You can also sort lists of strings.

```python
>>> strlist = ['a','d','c','b']
>>> strlist.sort()
>>> strlist
['a', 'b', 'c', 'd']
```

Or any type of predefined object for that matter[^1]. When sorting lists of lists, the interpreter looks at the average value of the lists.

```python
>>> listoflist = [[0, 1, 2, 3, 4], [5, 8]]
>>> listoflist.sort()
>>> listoflist
[[0, 1, 2, 3, 4], [5, 8]]
```

The one thing you cannot do is sorting a list with different types.

```python
>>> lis = ['extended2', 'extended1', 'appended', 4, 3, 2, 1, 'inserted']
>>> lis.sort()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'int' and 'str'
```

We could get around this by using the `key` parameter. Whatever function we give to the `key` parameter is applied to all objects and the list is only sorted afterwards.

```python
>>> lis.sort(key=str)
>>> lis
[1, 2, 3, 4, 'appended', 'extended1', 'extended2', 'inserted']
```

You might be confused to see the first four elements as integers not strings. Wasn't the key supposed to be applied to all elements? Yes, it was, but that's only temporary. The key is applied to each element and the sort is done according to the results but the original elements are the ones being sorted. So even when you define a key, only the order of the elements change, not the elements themselves.

To turn the order of sort, you can define the parameter `reverse` as `True`.

```python
>>> lis.sort(key=str, reverse=True)
>>> lis
['inserted', 'extended2', 'extended1', 'appended', 4, 3, 2, 1]
```

You can also use the function `sorted()` which returns a sorted version of the same function.

```python
>>> numlist = [5, 6, 8, 10, 22, 9, 3, 1, 2]
>>> sorted(numlist)
[1, 2, 3, 5, 6, 8, 9, 10, 22]
```

# List comprehension

Let's try making a function that checks if an element in list `l1` is also in list `l2`. And if it is, we'll remove it from `l1`. At this point of the course, I believe it'll be easy for you to imagine how we might do this. We just need to loop over the first list and check if the looped elements are in `l2` one by one.

```python
def removeDups(l1, l2):
	for e in l1:
		if e in l2:
			l1.remove(e)
```

Let's write a test suite and test our function.

```python
def testDups():
    l1 = [1, 2, 3, 4]
    l2 = [1, 2, 5, 6]
    removeDups(l1, l2)
    print('l1 =', l1)
    print(l1 == [3, 4])
```

When we run our `testDups()`,

```python
testDups()
```

the output is:

```
l1 = [2, 3, 4]
False
```

Wait a second. What's wrong? Well, the problem occurs when we try to change a list while we're looping over it. The indexing in a for loop only happens once.

| Index | Value | The current `l1` |
| ----- | ----- | ---------------- |
| `0`   | `1`   | `[2, 3, 4]`      |
| `1`   | `3`   | `[2, 3, 4]`      |
| `2`   | `4`   | `[2, 3, 4]`      |

So while the numbers shift, the indices stay the same, and this causes a mismatch between elements. How can we prevent this? We'll use **list comprehension**.

List comprehension offers a shorter syntax when we want to play with lists. For example to achieve what we tried with `removeDups(l1, l2)` we can just do:

```python
>>> l1 = [1, 2, 3, 4]
>>> l2 = [1, 2, 5, 6]
>>> [element for element in l1 if element not in l2]
[3, 4]
```

Translating this into English: "Give me that element for each element in `l1` if element isn't in `l2`."

This syntax is also useful for creating dictionaries or tuples or what have you. For example we can try creating a dictionary where keys are numbers from a list and values are `True` or `False` depending on whether the numbers are even or not.

```python
>>> l = [0, 1, 2, 3, 4, 5]
>>> { k:not bool(k%2) for k in l }
```

# Map and Lambda

Let's start with `map()`. `map()` is a function that, as it's name, maps a function to a group of objects. For example if I wanted to apply the function `int()` to each element in list `['1','2','3','4']` then I would simply do:

```python
>>> map(int, ['1', '2', '3', '4'])
<map object at 0x7300d211ffd0>
```

As you notice, this gave us a map object instead of a list, but not to worry; we can do whatever we want with this map object as it's an iterable. So for example:

```python
>>> for i in map(int, ['1', '2', '3', '4']):
...     print(i)
... 
1
2
3
4
```

Or if you don't like not seeing what you have clearly, you can just convert your map object into a list:

```python
>>> list(map(int, ['1', '2', '3', '4']))
[1, 2, 3, 4]
```

That's it for `map()`. Now what does `lambda`  have to do with all these? `lambda` is used to create functions. Often thought of as anonymous function creator, it actually adds no functionality and is only a shorthand form of what we regularly do to define functions. So for example think about the function \\( f(x)=x^2 \\). One way to define it might be

```python
>>> def c(x):
...     return x**2
...
```

But another way to define it is

```python
>>> f = lambda x: x**2
```

So to define a function (_lazily_) we can just use the syntax `lambda <input>:<output>`. One thing to note here is that the name of the second function isn't `f`. `f` there is just a variable we use to reference an _anonymous function_. 

Any function you define, when you try to print it, gives you the function name:

```python
>>> c
<function c at 0x75f4967a21e0>
```

But with `lambda` functions, there is no name to be given

```python
>>> f
<function <lambda> at 0x75f4967a2158>
```

These `lambda` expressions become useful when we want to map a custom function but we're too lazy to define it somewhere in our code (or we just want to keep our code simple)

```python
>>> sqrtmap = map(lambda x:int(x)**2, ['1', '2', '3', '4'])
>>> list(sqrtmap)
[1, 4, 9, 16]
```

## [Homework]({% post_url /homework/2020-12-19-homework-three %})

[^1]: _\_lt__ and _\_eq__ magic methods have to be defined for an object type to be sortable. We'll get to this later when we look at how to create our own object types. 