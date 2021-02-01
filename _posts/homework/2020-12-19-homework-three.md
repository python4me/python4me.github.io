---
title: Homework 3
author: Guess Who
date: 2020-12-19 14:36:00 +0300
categories: [Python, Lectures, Homework]
tags: [homework]
math: false
---

Answers to all of these questions will be available at [solutions]({% post_url /homework-solutions/2020-12-19-homework-solution-three %}) 24 hours before the next lecture.

# 1

Make a function `factorial(x)` that will return x! (the factorial of x)

```python
>>> def factorial(x):
    ...
    ...
>>> factorial(2)
2
>>> factorial(5)
120
```

# 2

Make the same `factorial(x)` function but use recursion this time.

# 3

Make a function `divisible(a, n)` that takes an iterable `a` and an integer `n` and then creates a list with the elements in `a` that are divisible by `n`. For example:

```python
>>> def divisible(a, n):
    ...
    ...
>>> divisible([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 3)
[3, 6, 9]
>>> >>> divisible([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 4)
[4, 8]
```

# 4

It was a dark and stormy night. Detective Havel and Hakimi Torrence arrived at the scene of the crime.

Other than the detectives, there were 10 people present. They asked the first person, "out of the 9 other people here, how many had you already met before tonight?" The person answered "5". They asked the same question of the second person, who answered "3". And so on. The 10 answers they got from the 10 people were:

```
5 3 0 2 6 2 0 7 2 5
```

The detectives looked at the answers carefully and deduced that there was an inconsistency, and that somebody must be lying. (For the purpose of this challenge, assume that nobody makes mistakes or forgets, and if X has met Y, that means Y has also met X.)

Your challenge for today is, given a sequence of answers to the question "how many of the others had you met before tonight?", apply the Havel-Hakimi algorithm to determine whether or not it's possible that everyone was telling the truth.

For a given sequence:
1. Remove all 0's from the sequence (i.e. warmup1).
2. If the sequence is now empty (no elements left), stop and return true.
3. Sort the sequence in descending order (i.e. warmup2).
4. Remove the first answer (which is also the largest answer, or tied for the largest) from the sequence and call it N.
5. The sequence is now 1 shorter than it was after the previous step.
6. If N is greater than the length of this new sequence (i.e. warmup3), stop and return false.
7. Subtract 1 from each of the first N elements of the new sequence (i.e. warmup4).
8. Continue from step 1 using the sequence from the previous step.

```python
>>> def hh(l):
    ...
    ...
>>> hh([5, 3, 0, 2, 6, 2, 0, 7, 2, 5])
False
>>> hh([3, 1, 2, 3, 1, 0])
True
```

# 5

Make a functional calculator.


