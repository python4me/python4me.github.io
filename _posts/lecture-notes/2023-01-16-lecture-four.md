---
title: Lecture Notes 4
author: Guess Who
date: 2023-01-16 13:36:00 +0300
categories: [Python, Lectures, Notes]
tags: [lecture]
math: true
---

A function, different to a mathematical one, is just a piece of code that only runs when it's called and returns a value if desired.

# Modules

We can just import our files from other files.

Modules will introduce you to the much much bigger world of python. The thing is, with basic python, the things you can do easily are limited. Of course, it's possible to do anything you want to do, but do you really want to spend 5 years to make and optimize a web framework so that you may make a website? This is where modules come in. Modules are code that other people wrote which you can `import` into your own code. Some modules are pre-installed while others need to be installed by you before using them. An example to the pre-installed modules is the `math` module.

```python
>>> import math
```

And to access variables defined in the `math` module we can do

```python
>>> math.pi
3.141592653589793
>>> math.e
2.718281828459045
>>> math.sqrt(2)
1.4142135623730951
```

Easy as that! I'm not going to get into the majority of available modules in these lessons because there are infinite amount of modules[^2], but one last thing I should tell you is another syntax for using modules. Just before, we needed to reference the imported module when we wanted to reference variables or the `sqrt()` function. How can we make this go away? By using `from ... import ...` 

`from ... import ...` will allow us to import specific functions or variables.

```python
>>> from math import pi
>>> pi
3.141592653589793
>>> from math import sqrt
>>> sqrt(2)
1.4142135623730951
>>> from math import e
>>> e
2.718281828459045
```

If we want to import all of the variables, objects, classes, anything that's in our module, we can use `*`

```python
>>> from math import *
>>> pi
3.141592653589793
>>> e
2.718281828459045
>>> sqrt(2)
1.4142135623730951
```

# Introduction to File Handling

File handling in python is simple and easy so I just want to get it "out of our way." 

To open a file simply do 

```python
>>> f = open('test')
```

now the variable `f` refers to the file `test` that is in the same directory as our script. Depending on your operating system you can give the open command a relative directory (such as `../test`).

One thing to note about file handling is that in python, there are different modes for file handling: reading / writing / appending. They all do exactly what they say[^1]. You define the mode after your filename as a string:

```python
f = open('test', 'r')
```

for reading,

```python
f = open('test', 'w')
```

for writing, and

```python
f = open('test', 'a')
```

for appending.

While you can't read a file if you open it in writing or appending mode, you also can't write to a file if you open it in reading mode. To read a file you should use the `read()` method:

```python
>>> data = f.read()
'hello there\nhello\tthere'
```

now all the content in our `test` file is stored in the `data` variable, but as you see, this will read a file into a single string while the original file looks like:

```
hello there
hello	there
```

If we wanted to read our file into a list of lines we could use the `readlines()` method instead:

```python
>>> data = f.readlines()
['hello there\n', 'hello\tthere']
```

To close our file we do

```python
>>> f.close()
```

Now let's try opening our file back up and writing something in it

```python
>>> f = open('test', 'w')
```

To write something in our file, similar to reading, we use the `write()` method.

```python
>>> f.write('roswell that ends well')
```

and then we can close and reopen our file to check what we have written: 

```python
>>> f.close()
>>> f = open('test', 'r')
>>> f.read()
'roswell that ends well'
```

Now lastly let's try appending something to this file and check it. The only difference between appending and writing is the mode string. Other than that, they're completely identical.

```python
>>> f.close()
>>> f = open('test', 'a')
>>> f.write('\n...or does it?')
>>> f.close()
>>> f = open('test', 'r')
>>> f.read()
'roswell that ends well\n...or does it?'
>>> f.close()
```

f.seek(0, 0) beginning
f.seek(0, 1) current
f.seek(0, 2) end

If you don't like this step-by-step approach to file handling you can use the `with open(...) as f:` syntax.

```python
>>> with open('test') as f:
...     f.read()
...
'roswell that ends well\n...or does it?'
```

Using this syntax, we don't have to ever use `f.close()`. The interpreter will close that file when it comes to the end of our `with` statement.

**DISCLAIMER:** In all these examples, we've run the commands in the python shell, therefore all the values were automatically printed to our screen. I'd like to take a moment to remind you that this won't be the case for your actual programs; you'll need to use `print()`, like always, to print things to your screen.

# F strings

You might've noticed how I use `f'string'` instead of just `'string'` in some solutions. These type of strings are called, appropriately, f-strings. F-strings allow us to write code in strings and easily stylize what we print. For example if we have a list and want to print each element with their index like:

```python
0: moo
1: foo
2: bar
```

We can do

```python
lis = ['moo', 'foo', 'bar']
print("0: "+lis[0]+"\n"+"1: "+lis[1]+"\n"+"2: "+lis[2])
```

But this is too much of a hassle isn't it? We could simply do

```python
print(f'''
0: {lis[0]}
1: {lis[1]}
2: {lis[2]}
''')
```

So anything we write inside curly braces `{}` in an f-string is evaluated and the value of it is turned into a string. 

You should also note how I made a multi-line string here. Multi-line strings are made with `'''` or `"""`.

# Git

Git is a way for people to collaborate on projects. Basically, git works in the following way:

![](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fwww.edureka.co%2Fblog%2Fwp-content%2Fuploads%2F2016%2F11%2FGit-Architechture-Git-Tutorial-Edureka-2.png&f=1&nofb=1)

1. Initializing a local repository with `git init`
2. Adding your files to the repository index with `git add`
3. Committing your files to the repository with `git commit -m <your message>`
4. Add the link to your remote repository in GitHub, GitLab etc. with `git remote add origin <link to your repo`
5. Push your local repository to your remote repository with `git push origin master`.
6. If there are changes in your remote repository, sync your local repository with `git pull origin master`
7. Create a new branch with `git branch <branch name>`
8. Switch to your branch with `git checkout <branch name>`
9. Merge your new branch into the master branch with `git merge <branch name>`

# Except handling

# Assertions

# (Real) Introduction to Object Oriented Programming

In this part I'll try to get you familiarized with classes. We use classes to, in simple terms, create custom types for our objects.

## Example 1

Let's create an `Employee` class. We want each object belonging to this class to have a `name` attribute. How might we do this?

First we make our class 3 46

```python
class Employee:
```

Then we need to define an `__init__` method. An `__init__` method defines the _initial state_ of instances (objects belonging to our class). So in our case, the `__init__` method will define the `name` attribute of each object.

```python
	def __init__(self, name):
        self.name = name
```

In object oriented programming the `self` attribute we pass into our methods refers to the object that is being created. So in this `__init__` method we're saying: "When you create an object, make that object's `name` attribute the value that is passed as a parameter when creating the object." To create an object of this type we'll do

```python
myObject = Employee('Jackson')
```

now we could try checking the type and name of our object

```python
print(f'''The type of our object is {type(myObject)}
The name of our object is {myObject.name}''')
```

If we run our script we'll get

```
The type of our object is <class '__main__.Employee'>
The name of our object is Jackson
```

You might be asking "What's that `__main__` all about? I thought our class' name was Employee." That isn't something you need to worry about. `__main__` refers to our script, the _main_ code that's running. This prefix is just there so we can understand where our class came from.

[^1]: You might be wondering the difference between writing and appending. It's simple. Every time you open a file to write, the pointer (that line you see when you're typing something) is at the very beginning of the file. So anything you write in the writing mode will override the existing content in your file. Appending is there to prevent this from happening. Whenever you open a file in appending mode, the pointer will be at the very end of your file, so nothing will get overwritten.
[^2]: But we'll touch up on some modules we can use for making websites, mobile apps, cryptographic tools later. 

# Homework

# 1

As a group, make a hangman game, using git and classes.
