---
title: Lecture Notes 2
author: Guess Who
date: 2020-12-12 13:36:00 +0300
categories: [Python, Lectures, Notes]
tags: [lecture]
math: true
---

I have said we'd go into lists et al. in a later lecture and here we are. **Iteration** as its name is about _iterating_. You have an object with multiple (or just one) elements and you cycle over them (or it). And in the last lecture I have mentioned to you that while some objects could be turned into others, some may not. This was--in essence--about whether an object was iterable or not. Lists and sets both have elements and they can be translated into one another. So can tuples. But though this rule doesn't always work (see: list to string translation is not that easy) but it's a really good starting point. The iterable object types are: `list`; `tuple`;`str` and a new one for you, `dict`.

# The `while` Loop

So far the names of the "stuff" we saw in python were all in accordance with the job of that "stuff." And while is no different. `while` runs the indented code while a statement is `True`. Let me show you with an example:

## Example 1

```python
while True:
    print('ikbal')
```

this will  give us

```
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
ikbal
...
```

and will not stop ever.

### How it works

1. The interpreter comes to the  `while` statement. 
2. The interpreter checks whether the statement after `while` is true, that is, it checks whether `True` is `True`.
3. `True` **is** true so the interpreter runs the indented part.
4. After running the indented part once, the interpreter checks again  whether the statement is true and it still is so it runs the code again
5. The interpreter continues to "check and run" until the statement becomes `False` so in this case, forever. Since `True` will never become `False`.

Let's see another, more extensive example:

## Example 2

```python
x = 3
while x < 10:
    x += 1
    print(x)
```

Output:

```python
4
5
6
7
8
9
10
```

### How it works

1. The interpreter comes to the  `while` statement. 
2. The interpreter checks whether `x < 10` is `True`.
3. We defined `x` to be 3 so the statement is True and the interpreter will run the indented code. 
4. The interpreter increases the value of x by 1 and prints the increased value.
5. After running the indented part, the interpreter checks whether the statement is still `True`. `x == 4` and `x < 10`, so it will run the indented part. 
6. This "check-run" process will continue until `x == 10`. When `x`'s value is 10, the statement will return `False` and the interpreter won't run the indented part.

# The `for` Loop

The syntax for `for` loops is the same with `while`. We use these loops when we want to do something with variables in sequences. The syntax is

```python
for element in iterable:
    <do something>
```

As you see, this is very human-readable. For each element in our iterable, we're telling the interpreter to "do something."

## Example 1

Let's try this with a simple list.

```python
for x in [0, 1, 2]:
    print(x)
```

One thing you should note here is `x`. This is just an arbitrary variable we have named. This means for each loop, the element in our list will be assigned to `x`. To put it in perspective

| Loop | Value of x |
| ---- | ---------- |
| 1st  | 0          |
| 2nd  | 1          |
| 3rd  | 2          |

And now we can run it:

```python
0
1
2
```

### What happened?

1. The first element in our list, `0`, gets assigned to `x`
2. The interpreter prints `x` which is no `0`
3. The second element in our list, `1`, gets assigned to `x`. 
4. `x`, which is now `1`, gets printed.
5. The last element in our list, `2`, gets assigned to `x`.
6. `x`, which is now `2`, gets printed.
7. There are no elements left in our list, so the `for` loop terminates.

## Example 2

Let's make a for loop with strings now. 

```python
for letter in "ikbal":
    print(ord(letter))
```

`ord()`:  Given a string representing one Unicode character, return an integer representing the Unicode code point of that character. For example, `ord('a')` returns the integer 97 and `ord(€)` returns 8364. This is the inverse of `chr()`. Here's an `ascii` table:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/ASCII-Table-wide.svg/875px-ASCII-Table-wide.svg.png)

Again, notice how our variable is now `letter`. In any case, you can name it whatever you want, but try to use understandable variables as much as you can.

Output:

```python
105
107
98
97
108
```

### What happened?

1. The first letter of `'ikbal'` (`'i'`) gets assigned to the `letter` variable.
2. The `ascii` code of `'i'` is printed.
3. Repeat step 1 and 2 for the second, third, forth and fifth letter.
4. The loop terminates since there are no letters left.

## Example 3

Let's try to make something useful this time: a function that will take two inputs `n` and `num` and return the positive nth root of `num`. Let's also limit `num` to only be positive and of the form `answer**n`. For example if `n == 2` then `num` is a perfect square.

```python
>>> def nth_root(n, num):
    ...
    ...
>>> nth_root(2, 9)
3
>>> nth_root(2, 4)
2
>>> nth_root(3, 27)
3
```

The method will use here is called **exhaustive enumeration**. It's basically trial and error. The process is as follows:

1. Make an `answer` variable and give it the initial value of `1`.
2. Take our `answer` to the nth power and check if it is equal to our `num`.
3. If it isn't, increase the value of our `answer` and repeat the second step until we get our answer.

 You can try to implement this on your own before checking the answer.

Starting with the definition line:

```python
def nth_root(n, num):
```

We indent and define our `answer` variable. Since our `num` is limited to positive perfect nth powers and `answer` has to be positive, `1` is a good value to start with.

```python
    answer = 1
```

We now need to make a loop which will increase our `answer` if `answer**n != num` (if the nth power of our current `answer` is not equal to `num`).

```python
    while answer**n != num:
        answer += 1
```

After this loop completes, `answer` variable will be equal to the actual nth root of `num`. So we can just return it now.

```python
    return answer
```

Let's test our function a bit.

```python
print(nth_root(3, 27))
print(nth_root(4, 16))
print(nth_root(10, 9765625))
```

Output:

```
3
2
5
```

### The smarter way

We know that \\(\sqrt[n]{x}=x^{1/n}\\) so that's what we'll use. (You can also use \\(\sqrt[n]{x}=x^{n^{-1}}\\) if you want to)

Define our function.

```python
def smartroot(n, num):
    return num**(1/n)
```

Testing lines:

```python
print(smartroot(3, 27))
print(smartroot(4, 16))
print(smartroot(10, 9765625))
```

Output:

```
3.0
2.0
5.000000000000001
```

Oh! What's going on? This folks, is why we don't mess with `float`s. Please check [What Every Programmer Should Know About Floating-Point Arithmetic](https://floating-point-gui.de/) and especially [this](https://floating-point-gui.de/basic/) page. Done? No? Alright here's a quote from the page:


> #### Basic Answers
>
> Why don’t my numbers, like 0.1 + 0.2 add up to a nice round 0.3, and instead I get a weird result like 0.30000000000000004?
>
> Because internally, computers use a format (binary floating-point) that cannot accurately represent a number like 0.1, 0.2 or 0.3 at all.
>
> When the code is compiled or interpreted, your “0.1” is already rounded to the nearest number in that format, which results in a small rounding error even before the calculation happens.
>
> #### Why do computers use such a stupid system?
>
> It’s not stupid, just different. Decimal numbers cannot accurately represent a number like 1/3, so you have to round to something like 0.33 - and you don’t expect 0.33 + 0.33 + 0.33 to add up to 1, either - do you?
>
> Computers use binary numbers because they’re faster at dealing with those, and because for most calculations, a tiny error in the 17th decimal place doesn’t matter at all since the numbers you work with aren’t round (or that precise) anyway.

And here's a relevant comic:

![](https://imgs.xkcd.com/comics/e_to_the_pi_minus_pi.png)

## Example 4

The whole point of this example is to have an excuse to introduce you to the `range()` function. OK I lied. Range is not really a function. It's actually a type on it's own called `range`.

```python
>>> type(range(5))
<class 'range'>
```

For now, it's enough for you to know that `range` is much like `tuple`s (Remember the un-changeable lists from last lecture?) Also that range creates a sequence with the following properties:

```python
range(start, stop, step)
```

`start`: the first number in our range (defaults to `0`)
`stop`: range stops _before_ this index. ([What?!](#why-range-stops-before))
`step`: this is the common difference between elements in our range. (defaults to `1`)


Let's move on with our example and you'll understand it more clearly.

```python
for number in range(0, 5, 1):
    print(number)
```

```
0
1
2
3
4
```

So you see, this is equivalent to using the list  `[0, 1, 2, 3, 4]` the only difference is that it's more versatile. In fact

```python
>>> list(range(0, 5, 1))
[0, 1, 2, 3, 4]
```

For a sub-example let's try changing the step.

```python
for i in range(0, 10, 2):
    print(i)
```

```
0
2
4
6
8
```

Here you can see that the `step` is quite literally "the step." It's how much our values increase in each new element. There's one more cool thing we can do with `range()` out of the box and that's reverse ranges.

```python
for i in range(10, 0, -3):
    print(i)
```

```
10
7
4
1
```

Another thing that might be apparent is that because of the defaults of `start` and `step` . You can use `range()` without those two and still make it work. (They'll be added in for you.)

```python
for i in range(5):
    print(i)
```

```
0
1
2
3
4
```

### Why range stops before

Remember how we indexed our elements in a list or string?

```
index: 0  1  2  3
List: [0, 1, 2, 3]
```

So **arrays start at 0 (zero).** Let's take the `range(5)` for example and turn it into a list:

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```

You remember I said the range stops at this index. And you can see it clearly here. The range stopped before the 5th index which is `5`.  And by that, since the range starts at `0`, it stopped before getting to the actual number `5`. Practically, when defining a `range` (if you don't define `start` and `stop`) you may think that the `stop` is equal to the number of integers you get as a result. 

It's, for this reason, better to think that range has a sea of infinite integers that go both ways. And when we define a `range` object we're just taking a slice between our specified indexes.

# `break` and `continue`

Like we can make loops, we can also break them and `break` does just that.

## Example 1

```python
x = 0
while True:
    print(x)
    x += 1
    if x == 6:
        break
        print("Terminated loop.")
```

```
0
1
2
3
4
5
```

Here the interpreter increased `x`'s value until `x == 6` in which case `break` ran. The thing you must take note here is that, the last `print()` function didn't run.

`continue` is much like break but what it does is not end the iteration altogether but just skip the current loop.

## Example 2

```python
for letter in 'string':
    if letter == 'r':
        continue
    print(letter)
```

```
s
t
i
n
g
```

What happened here was when the loop came to the `r` character, the `letter == 'r'` condition was fulfilled and the interpreter ran `continue` which skipped the letter `r` and _continued_ to the letter `i`.

# Dictionaries

`dict` is (not-so-surprisingly) the dictionary object. Dictionaries in python work the same way they do in real life. You have a term and you have a definition. The only difference here is that terms are called `keys` and definitions are called `values`. 

```python
>>> type({'ikbal': 'pretty'})
<class 'dict'>
```

Although you don't need your values and keys to be `str`. They can be numbers, lists or anything you want (even custom objects but that's for later!). And of course like any good dictionary you can have multiple key-value pairs in your dictionary.

```python
my_dict = {'ikbal': 'pretty',
           'elif': 'not pretty'}
```

The last thing to know about dictionaries is overriding previous pairs. The last definition will always override the first definition you write. For example

```python
my_dict = {'ikbal': 'pretty',
           'elif': 'not pretty',
           'ikbal': 'not pretty',
           'elif': 'pretty'}
```

If we print this list we'll get

```
{'ikbal': 'not pretty', 'elif': 'pretty'}
```

We can refer to values in a dictionary with the following syntax:

```python
dictionary[key]
```

## Example

```python
grades = {'John': 100, 'Jake':85, 'Jolene':60}
```

If we want to get John's grade, all we need to do is

```python
johns_grade = grades['John']
print(johns_grade)
```

Output:

```
100
```

Let's talk about how to iterate over this dictionary of ours. If we want to iterate over just the keys

```python
for key in grades:
    ...
    ...
```

If we want to have both the keys and the values

```python
for key, value in grades.items():
    ...
    ...
```

# Slicing and Indexing

We're finally here. This section will cover all the ways you can manipulate `str`, `list`, `tuple` or what have you.

## Indexing

Syntax:

```python
object[index]
```

Yup. That's all there is to it. Doesn't matter which object either. If it's index-able this is they way to do it.

## Example 1

```python
string = "index me coward. you're only gonna index a string."
```

Let's take the letter at the 4th index of this string.

```python
indexed = string[4]
print(indexed)
```

Output:

```
'x'
```

Done!

## Example 2

```python
lis = [0, 1, 2, 3]
```

Let's index this list and get the element at 2nd index.

```python
indexed = lis[2]
print(indexed)
```

Output:

```
2
```

Done!

Tuples are exactly the same as these two and the syntax never changes here, so I'll pass that.

## Slicing

Syntax:

```python
object[start:stop:step]
```

This is much like how we define our ranges.

`start`: the first index in our slice (defaults to "the rest")
`stop`: range stops _before_ this index. (defaults to "the rest")
`step`: this is the common difference between elements in our slice. (defaults to `1`)

### Example 1

```python
string = "this is a long string me thinks"
```

Let's try to take the first word of this string. Our word `this` starts at the `0th` index and end before the `4th` index. So that's what we'll use. We don't have to define our `step` since that's `1` by default.

```python
sliced = string[0:4]
print(sliced)
```

Output:

```
'this'
```

#### Smarter way

There is a string method called `split()` that can ease our job immensely here. `split()` does just what it says, it splits. The syntax is

```python
string.split(which_character_to_split, how_many_times)
```

 By default `split()` splits by `' '` (space character) and as many times it can. So for example in our case this would be

```python
>>> splitted = string.split()
>>> print(splitted)
['this', 'is', 'a', 'long', 'string', 'me', 'thinks']
```

And then we could just get the first element of this list with

```python
>>> splitted[0]
'this'
```

### Example 2

```python
string = "this is a long string me thinks"
```

Let's try first get every character before the 6th index and then get every character after and including the 6th index.

```python
before6 = string[:6]
after6 = string[6:]
print("Every char < 6th:", before6)
print("Every char >= 6th:", after6)
```

Output:

```
Every char < 6th: this i
Every char >= 6th: s a long string me thinks
```

### Example 3

```python
lis = [0, 1, 2, 3, 4, 5, 6]
```

Let's get the elements between 1st and 5th indices.

```python
sliced = lis[2:5]
print(sliced)
```

Notice how we don't want the 1st index so we define `start` as 2.

Output:

```
[2, 3, 4]
```

### Example 4

```python
tup = (0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13)
```

Let's get the every 5th element in this tuple

```python
every5th = tup[::5]
print(every5th)
```

Notice here how we used the defaults for `start` and `stop` which were "the rest." This will ensure that we start at the beginning of our tuple and end at the end.

Output:

```
(0, 5, 10)
```

# One last note about iterable types

This part is to introduce you to the `len()` (length) function. It simply gives us how many elements there are in an iterable.

```python
>>> len([0, 1, 2, 3, 4, 5])
6
>>> len('string')
6
```
# [Homework]({% post_url /homework/2020-12-12-homework-two %})