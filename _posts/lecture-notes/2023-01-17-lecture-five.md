---
title: Lecture Notes 5
author: Guess Who
date: 2023-01-17 13:36:00 +0300
categories: [Python, Lectures, Notes]
tags: [lecture]
math: true
---

```python
# Raising forced exceptions
x = 10
if x > 5:
    raise Exception('x should not exceed 5. The value of x was: {}'.format(x))

# assert
import sys
assert ('linux' in sys.platform), "This code runs on Linux only."

#try except


def linux_interaction():
    assert ('linux' in sys.platform), "Function can only run on Linux systems."
    print('Doing something.')
try:
    linux_interaction()
except:
    print('Linux function was not executed')

# printing out the error
try:
    linux_interaction()
except AssertionError as error:
    print(error)
    print('The linux_interaction() function was not executed')

# file opening error
try:
    with open('file.log') as file:
        read_data = file.read()
except:
    print('Could not open file.log')


# composite errors
try:
    linux_interaction()
    with open('file.log') as file:
        read_data = file.read()
except FileNotFoundError as fnf_error:
    print(fnf_error)
except AssertionError as error:
    print(error)
    print('Linux linux_interaction() function was not executed')

# else after except
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    print('Executing the else clause.')

# try inside else
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)

# clean up with finally
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
finally:
    print('Cleaning up, irrespective of any exceptions.')

# enumerate

#regular
example = ['left','right','up','down']
for i in range(len(example)):
    print(i, example[i])
#with enumerate
for i,j in enumerate(example):
    print(i,j)

# zip

x = [1,2,3,4]
y = [7,8,3,2]
z = ['a','b','c','d']

#two arguments
for a,b in zip(x,y):
    print(a,b)

# three arguments
for a,b,c in zip(x,y,z):
    print(a,b,c)

#zip object
print(zip(x,y,z))

#zip object to list
print(list(zip(x,y,z)))

#zip object to dict
names = ['Jill','Jack','Jeb','Jessica']
grades = [99,56,24,87]

d = dict(zip(names,grades))
print(d)

#dont name variables stupidly
x = [1,2,3,4]
y = [7,8,3,2]
z = ['a','b','c','d']

[print(x,y,z) for x,y,z in zip(x,y,z)]

#[print(x,y,z) for x,y,z in zip(x,y,z)]

for x,y,z in zip(x,y,z):
    print(x,y,z)

# generators

def simple_gen():
    yield 'Oh'
    yield 'hello'
    yield 'there'

for i in simple_gen():
    print(i)

# basic bruteforce
CORRECT_COMBO = (3, 6, 1)

for c1 in range(10):
    for c2 in range(10):
        for c3 in range(10):
            if (c1, c2, c3) == CORRECT_COMBO:
                print('Found the combo:{}'.format((c1, c2, c3)))

for c1 in range(10):
    for c2 in range(10):
        for c3 in range(10):
            if (c1, c2, c3) == CORRECT_COMBO:
                print('Found the combo:{}'.format((c1, c2, c3)))
                break

found_combo = False
for c1 in range(10):
    if found_combo:
        break
    for c2 in range(10):
        if found_combo:
            break
        for c3 in range(10):
            if (c1, c2, c3) == CORRECT_COMBO:
                print('Found the combo:{}'.format((c1, c2, c3)))
                found_combo = True
                break

def combo_gen():
    for c1 in range(10):
        for c2 in range(10):
            for c3 in range(10):
                yield (c1, c2, c3)

for (c1, c2, c3) in combo_gen():
    print(c1, c2, c3)
    if (c1, c2, c3) == CORRECT_COMBO:
        print('Found the combo:{}'.format((c1, c2, c3)))
        break

#generator expressions
xyz = (i for i in range(50000000))
print(list(xyz)[:5])

# sys.argv

sys.argv # gives list of all arguments passed to the python commandline

sys.argv[0] # name of the script

import sys

opts = [opt for opt in sys.argv[1:] if opt.startswith("-")]
args = [arg for arg in sys.argv[1:] if not arg.startswith("-")]

if "-c" in opts:
    print(" ".join(arg.capitalize() for arg in args))
elif "-u" in opts:
    print(" ".join(arg.upper() for arg in args))
elif "-l" in opts:
    print(" ".join(arg.lower() for arg in args))
else:
	# sys.exit()
    raise SystemExit(f"Usage: {sys.argv[0]} (-c | -u | -l) <arguments>...")

# argparse
import argparse

parser = argparse.ArgumentParser(description='Process some integers.')
parser.add_argument('integers', metavar='N', type=int, nargs='+',
                    help='an integer for the accumulator')
parser.add_argument('--sum', dest='accumulate', action='store_const',
                    const=sum, default=max, help='sum the integers (default: find the max)')

args = parser.parse_args()
print(args.accumulate(args.integers))

# Timeit
# importing the required module
import timeit
 
# code snippet to be executed only once
mysetup = "from math import sqrt"
 
# code snippet whose execution time is to be measured
mycode = '''
def example():
    mylist = []
    for x in range(100):
        mylist.append(sqrt(x))
'''
 
# timeit statement
print (timeit.timeit(setup = mysetup,
                     stmt = mycode,
                     number = 10000))
# importing the required modules
import timeit
 
# binary search function
def binary_search(mylist, find):
    while len(mylist) > 0:
        mid = (len(mylist))//2
        if mylist[mid] == find:
            return True
        else if mylist[mid] < find:
            mylist = mylist[:mid]
        else:
            mylist = mylist[mid + 1:]
    return False
 
 
# linear search function
def linear_search(mylist, find):
    for x in mylist:
        if x == find:
            return True
    return False
 
 
# compute binary search time
def binary_time():
    SETUP_CODE = '''
from __main__ import binary_search
from random import randint'''
 
    TEST_CODE = '''
mylist = [x for x in range(10000)]
find = randint(0, len(mylist))
binary_search(mylist, find)'''
     
    # timeit.repeat statement
    times = timeit.repeat(setup = SETUP_CODE,
                          stmt = TEST_CODE,
                          repeat = 3,
                          number = 10000)
 
    # printing minimum exec. time
    print('Binary search time: {}'.format(min(times)))       
 
 
# compute linear search time
def linear_time():
    SETUP_CODE = '''
from __main__ import linear_search
from random import randint'''
     
    TEST_CODE = '''
mylist = [x for x in range(10000)]
find = randint(0, len(mylist))
linear_search(mylist, find)
    '''
    # timeit.repeat statement
    times = timeit.repeat(setup = SETUP_CODE,
                          stmt = TEST_CODE,
                          repeat = 3,
                          number = 10000)
 
    # printing minimum exec. time
    print('Linear search time: {}'.format(min(times))) 
 
if __name__ == "__main__":
    linear_time()
    binary_time()


def f(x):
    return x**2
def g(x):
    return x**4
def h(x):
    return x**8

import timeit
print(timeit.timeit('[func(42) for func in (f,g,h)]', globals=globals()))


# Multiprocessing

#uses max 15%
while True:
    print('Spawned')

#still not much
import multiprocessing

def spawn():
    print('Spawned')

if __name__ == '__main__':
    for i in range(5):
        p = multiprocessing.Process(target=spawn)
        p.start()
        p.join()

#ooh baby
import multiprocessing

def spawn():
    print('Spawned')

if __name__ == '__main__':
    for i in range(100):
        p = multiprocessing.Process(target=spawn)
        p.start()

# they are not in order
import multiprocessing

def spawn(num):
    print('Spawn # {}'.format(num))

if __name__ == '__main__':
    for i in range(5):
        p = multiprocessing.Process(target=spawn, args=(i,))
        p.start()

#creating pools to return values from shared pool
from multiprocessing import Pool

def job(num):
    return num * 2

if __name__ == '__main__':
    p = Pool(processes=20)
    data = p.map(job, [i for i in range(20)])
    p.close()
    print(data)

# Asyncio

# Decorators

# Magic Methods

# Logging

# Debugging

# Args Kwargs

# Asyncio

# Search and Sort

# pathlib 
https://docs.python.org/3/library/pathlib.html
```
