---
title: Homework Solutions 1
author: Guess Who
date: 2020-11-26 13:36:00 +0300
categories: [Python, Lectures, Solutions]
tags: [solutions]
math: false
---

# Simple part

## The rules of the game

OK let's first think about how we can go about doing this. 

1. We have two separate arguments so  we need to turn them into writing one-by-one.

2. We need to check which number our input corresponds to and return a writing.

3. Instead of checking them one by one (`if input == 1`, `if input == 2` etc.) we can use lists
   - We'll make a list such that `our_list[number] == number in writing`. 

4. We need to check for the "weird" numbers in the English language between 10 and 20. (15 = fifteen and not ten five)

Alright with these rules let's start building our function. First we write the definition line and then we indent

```python
def time2words(hour, minute):
    #indented
```

## Wordlists

Now we need to make our wordlists. This may seem daunting since there are 60 different numbers we need to come up with but actually, we don't need to define them all. We can just define the ones (1,2,3,4...) and tens (10, 20, 30, 40...) and then we can use both of these lists at once to express numbers like 45 (forty + five).

```python
    ones = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['ten', 'twenty', 'thirty', 'forty', 'fifty']
```

But there's a slight problem here. Because this doesn't abide the way we had things in mind in our 3rd rule. We want to have a list so that `ones[1]=='one'` but here we clearly have `ones[1]=='two'` because lists start from 0. So we slightly adjust our list.

```python
    ones = ['zero', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['zero', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
```

But it's still not quite right. How might we express a time like 03:00? 3 am. So we don't even ever say the `'zero'` part. We just leave it blank. Let's change our lists to reflect this.

```python
    ones = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
```

But these lists will only be useful for the minute part (actually you can adjust them to be useful for the hours part too  but I'm not gonna do that so this solution is not too complicated to follow.) We need to make a list of hours too. How about

```python
    hours = ['one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
```

This is good but again we need to modify it so we can have `hours[hour]==hour in writing`.

```python
    hours = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
```

The problem now is, 00:21 is "twelve twenty one am" and not "twenty one am" so we need to change our first item to be `'twelve'`. 

```python
    hours = ['twelve', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
```

With our wordlists in place, we can now move on to translation.

## Translating hours

Translating hours is very easy since we defined our lists "right".

```python
    hour_writing = hours[hour]
```

But this has a big problem. What about the hours after 12? Our function takes 24-hour format and outputs 12-hour format. And our `hours` list only has hours until 12. So if we try to do `hours[15]` then we'll get a nasty `IndexError`.

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

To prevent this is easy. Think about it. How do we humans know that 15 is 3? Practically we just subtract 12 from 15. So maybe we can do

```python
    if hour > 12:
        hour_writing = hours[hour-12]
    else:
        hour_writing = hours[hour]
```

This is a valid solution but can we make it better? What else gives us 3 from 15? How about `mod 12`? You see, `15%12=3` and `3%12=3` so if we use mod, we won't even need to branch. Let's replace the last part with

```python
    hour_writing = hours[hour%12]
```

So now what will happen? For example if we do `time2words(15, 30)` then `hour_writing == hours[15%12]`. `15%12` is 3 so `hours_writing` will be equal to the 3rd (4th in human terms) element of the `hours` list. The last thing for us to do in this section is to adjust the `am` or `pm` parts. How about

```python
	if hour > 12:
		appendix = 'pm'
	else:
		appendix = 'am'
```

This is good but like everything, it's not quite right. The problem is that 12:00 (`hour > 12`) is "twelve _pm_" and 00:00 (`hour < 12`) is "twelve _am_." With our current model 12:00 will be "twelve _am_" while 00:00 is "twelve _am_," and `time2words(24, 0)` is "twelve pm." We need to first check whether `hour == 12 or 24` and do things differently if that's the case.

```python
	if hour == 24:
		appendix = 'am'
	elif hour == 12:
		appendix = 'pm'
	elif hour > 12:
		# Note how this part won't even be checked if any of the statements above is checked.
		appendix = 'pm'
	else:
		appendix = 'am'
```

So now first the interpreter checks whether `hour==24` if it is, then it'll skip the others (which is good because we don't want the 3rd elif statement to run). Check [extra](#extra) for a better way to do this. This concludes the part on hours.

## Translating minutes

With minutes we have two parts. The tens part and the ones part. For example with 52 we have 50 + 2. How might we separate  50 and 2 from 52? Let's start with the ones part

### Ones

Here we need to discard the tens part. The easy way to do this is to take the `mod 10` of our minute. With this, we'll get `52%10==2` for example.

```python
    ones_minute = minute%10
```

Now we have the ones part captured in a variable, we can translate it to words. This translation will be done by finding the value in our ones list (just like we found the hour in hours list).

```python
	ones_minute_writing = ones[ones_minute]
```

The ones part is effectively over.

### Tens

Now how do we get the 50 from 52? Well we already have 2, so why don't we just subtract it to get the tens part? (52-2=50 which is what we want now)

```python
    tens_minute = minute-ones_minute
```

Now we can't translate this as easily as the ones part because `tens_minute[50]` doesn't exist. (We only have 6 elements in our tens list) So we need to take the 5 from 50. This is easy to do since we only need to divide our number by 10.

```python
	tens_minute_num = tens_minute/10
```

But no, this doesn't work. Because `tens_minute/10` is a float and not an integer. And we can't use floats as indices to lists. To fix this is easy

```python
    tens_minute_num = tens_minute//10
```

With integer division we'll always get an integer as a result. Now we need to translate this number to writing and we'll do it the way we did before.

```python
	tens_minute_writing = tens[tens_minute_num]
```

## Putting it all together

### Weird part  (10-20)

Putting this all together would be very simple if English wasn't so weird. But it is. With our current model 15 is "ten five" and not "fifteen." When we're returning our output, we need to take care of this with an if statement. 

So when the minute is between 10 and 20, we want to return a modified time.

```python
	if 10 < minute < 20:
```

We need to make a wordlist like we did for the hours and minutes, only this time it'll be for the "weird" words.

```python
	    weird = ['', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
```
(This part is double indented [8 spaces]) I'm sure you get why we make our first element `''` now. You see, we already have all the possible numbers between 10 and 20 in our list but `weird[number]` doesn't exist because our list doesn't have that many elements. How can we fix this? Well we can use our `ones_minute` variable we stored earlier. Because for example if we have 11 then `ones_minute == 1` and `weird[ones_minute]` is "eleven."

```python
	    weird_writing = weird[ones_minute]
```
At last we return our full string

```python
	    return hour_writing+' '+weird_writing+' '+appendix
```
But our work isn't over since we only defined what the function will return when there is a weird number. How about when there isn't one?

### Regular returning

We made the weird part with an if statement and it's only right that we continue this part with an else statement. But note that you don't actually need this. Because if the if part runs, then the function will `return ` something and interpreter won't read the rest of the function. (The interpreter doesn't read what's written in a function after it sees the `return` line)

```python
    else:
```
We can simply return what we have in order

```python
        return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

This will _work_. But if we care at all about aesthetics we got work to do. You see there's a possibility that `tens_minute_writing` or `ones_minute_writing` to be `''` in fact they can both be `''` at the same time and then we'll have too many spaces lying around. For example `time2words(15, 0)=='three   pm'`. But we can fix this with branching.

First let's think about what different minute types we can have

1. 00
2. X0
3. XY

Where X=Y is a possibility.

So remove the last thing we added. Now, we'll handle the case where both of our variables is `''` (case 1).

```python
	    if ones_minute_writing == '' and tens_minute_writing == '':
            return hour_writing+' '+appendix
```

Since our variables are empty strings we don't even need to include them. 

Now we'll handle the case where `ones_minute` is 0 but `tens_minute` isn't (case 2). If you know some logic then it'll be easy for you to understand this.

```python
        elif (ones_minute_writing == '') and (tens_minute_writing != ''):
```

And we return our string (this time adding 2 spaces - no need to add `ones_minute_writing` since that's just `''`.)

```python
            return hour_writing+' '+tens_minute_writing+' '+appendix
```

And our last case is the one we thought of first. This time neither `ones_minute` nor `tens_minute` is 0.

```python
        else:
		    return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

We can just use `else` here because we're out of options. Aaand we're done. That was it.

## Code in full w/ Comments

```python
def time2words(hour, minute):
	#define our words
	#we take the empty string first before because we want ones[1] = "one"
	#you dont need to really define your wordlist this way but this seems easier to read / easier to implement so we'll go with this
	#if time is 15:00 then it's just 3 am
	ones = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
	#Notice how ones[1] = 'one' and ones[4] = 'four' because ones[0] = ''


	#The GAME PLAN: We will find the hour in our lists and return it. Then we'll find the minute in our lists and return it.
	
	# HOUR PART
	hours = ['twelve', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
	# This list like all our lists is conveniently set up. Because hours[12] = 'twelve' and hours[2] = 'two'
	# So hours[hour] will give us hour in writing if the hour is lower than 12 but what happens if the hour is 15?
	# Then hours[15] does not exist and it will give us an error. What can we do?
	# We can divide by 12 and get the remainder. Meaning: We can use 'mod 12'. 15 mod 12 = 3 and 3 mod 12 is also 3
	# So this certainly fits in with what we want to do.
	hour_writing = hours[hour%12]

	# The last thing for us to do is to determine whether we are in the am or pm
	# This is rather simple. We just need to check whether hour is bigger than 12
	# Although we have a problem because 24.00 is 12 am not pm and 12.00 is 12 am not pm
	# We can fix this with multiple branches.
	if hour == 24:
		appendix = 'am'
	elif hour == 12:
		appendix = 'pm'
	elif hour > 12:
		# Note how this part won't even be checked if any of the statements above is checked.
		appendix = 'pm'
	else:
		appendix = 'am'
        
	# MINUTE PART
	# Find the ones part of our minute. 

	#When we get the remainder from diving a number by 10, we get the ones part.
	# For example: 52 = 5*10+2 so 52 mod 10 = 2.
	
	ones_minute = minute%10

	# Now we need to find the word that corresponds with this ones part. Luckily our list is conveniently set up.
	# So whatever one_minute might be, ones[ones_minute] is ones_minute in writing 
	# For example if minute=52 then ones_minute=2 and ones[2]='two'
	ones_minute_writing = ones[ones_minute]

	# Let's get the tens part now. We can do this by simply subtracting the ones_minute from minute
	tens_minute = minute-ones_minute

	# Unfortunately we can't do the same trick we did with ones_minute here to get the word part
	# Because we'll be getting tens[50] if our number is 52. But this can be easily solved
	# We'll just divide tens_minute by 10 to get the number part of it
	# So if tens_minute is 50, we'll get 5 by dividing that and tens[5]='fifty' so it will work out.

	tens_minute_num = tens_minute//10
	# You may find it odd that we use integer division here but the reason is that regular division will return a float
	# And we can't use floats as indices.
	tens_minute_writing = tens[tens_minute_num]

	# Putting it all together
	
	# Now we can simply return hour_writing, tens_minute_writing, ones_minute_writing
	# But our problem here is with the English language. Because 15 is "fifteen" not ten five.
	# We'll first make an if statement to check for this and fix our words accordingly.
	if 10 < minute < 20:
		# Make a list of our 'weird' words
		weird = ['', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
		# This list is also conveniently set up but it just needs a little work.
		# Notice how with our new list, weird[1] = 'eleven'
		# So we can just do weird[ones_minute] to get the word version of a number
		weird_writing = weird[ones_minute]
		#And now we'll return the full word part
		return hour_writing+' '+weird_writing+' '+appendix


	else:
		# Now we'll return the regular. Note that if the minute isn't between 10 and 20 we haven't returned anything yet.
		# return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
		# But we must be aware that if tens_minute_writing or ones_minute_writing is '' then there will be too many spaces
		# We can fix that with a nested if statement 
		if ones_minute_writing == '' and tens_minute_writing == '':
			# This will only work when both of the writings are ''
			return hour_writing+' '+appendix
		elif (ones_minute_writing == '') and (tens_minute_writing != ''):
            # This will work when ones_minute_writing is '' but tens_minute_writing isn't.
			return hour_writing+' '+tens_minute_writing+' '+appendix
		else:
			return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

## Code in full w/o Comments

```python
def time2words(hour, minute):
    ones = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
    hours = ['twelve', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
    
    hour_writing = hours[hour%12]
	if hour == 24:
		appendix = 'am'
	elif hour == 12:
		appendix = 'pm'
	elif hour > 12:
		appendix = 'pm'
	else:
		appendix = 'am'
        
    ones_minute = minute%10
	ones_minute_writing = ones[ones_minute]
    
    tens_minute = minute-ones_minute
    tens_minute_num = tens_minute//10
	tens_minute_writing = tens[tens_minute_num]
	if 10 < minute < 20:
	    weird = ['', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
	    weird_writing = weird[ones_minute]
	    return hour_writing+' '+weird_writing+' '+appendix
    else:    
	    if ones_minute_writing == '' and tens_minute_writing == '':
            return hour_writing+' '+appendix
        elif (ones_minute_writing == '') and (tens_minute_writing != ''):
            return hour_writing+' '+tens_minute_writing+' '+appendix
        else:
		    return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

## Extra

```python
    appendix = 'am'
    if 24 > hour >= 12:
	    appendix = 'pm'
```
Here we pre-define `appendix` to be `'am'` and override it when required (that is, when `hour` is between 24 and 12 or 12)

# Advanced part

For the advanced part only difference is that we need to find a way to convert our `time_string` to two variables: `hour` and `minute`. And like we're gonna see this week, this is done by slicing the string. We need to split by the `:` character. Since we know that there's only one, this will be easy.

```python
    time_list = time_string.split(':')
    hour = int(time_list[0])
    minute = int(time_list[1])
```
As you see, the `split` method is used to split strings into lists. So for example

```python
>>> time_string = '15:15'
>>> time_string.split(':')
['15', '15']
>>> time_list[0]
'15'
>>> time_list[1]
'15'
>>> int(time_list[0])
15
>>> int(time_list[1])
15
```

We just need to adjust our function's arguments and add this snippet to the beginning to complete the advanced part.

## Code in full w/ Comments

```python
def time2words(time_string):
	# PREPARATION
	# We'll just get the hour and minute variables by breaking up time_string.
	# We need to split by the ':' character. Since we know that there's only one, this will be easy.
	time_list = time_string.split(':')
	hour = int(time_list[0])
	minute = int(time_list[1])    
    
	#define our words
	#we take the empty string first before because we want ones[1] = "one"
	#you dont need to really define your wordlist this way but this seems easier to read / easier to implement so we'll go with this
	#if time is 15:00 then it's just 3 am
	ones = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
	#Notice how ones[1] = 'one' and ones[4] = 'four' because ones[0] = ''


	#The GAME PLAN: We will find the hour in our lists and return it. Then we'll find the minute in our lists and return it.
	
	# HOUR PART
	hours = ['twelve', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
	# This list like all our lists is conveniently set up. Because hours[12] = 'twelve' and hours[2] = 'two'
	# So hours[hour] will give us hour in writing if the hour is lower than 12 but what happens if the hour is 15?
	# Then hours[15] does not exist and it will give us an error. What can we do?
	# We can divide by 12 and get the remainder. Meaning: We can use 'mod 12'. 15 mod 12 = 3 and 3 mod 12 is also 3
	# So this certainly fits in with what we want to do.
	hour_writing = hours[hour%12]

	# The last thing for us to do is to determine whether we are in the am or pm
	# This is rather simple. We just need to check whether hour is bigger than 12
	# Although we have a problem because 24.00 is 12 am not pm and 12.00 is 12 am not pm
	# We can fix this with multiple branches.
	if hour == 24:
		appendix = 'am'
	elif hour == 12:
		appendix = 'pm'
	elif hour > 12:
		# Note how this part won't even be checked if any of the statements above is checked.
		appendix = 'pm'
	else:
		appendix = 'am'
        
	# MINUTE PART
	# Find the ones part of our minute. 

	#When we get the remainder from diving a number by 10, we get the ones part.
	# For example: 52 = 5*10+2 so 52 mod 10 = 2.
	
	ones_minute = minute%10

	# Now we need to find the word that corresponds with this ones part. Luckily our list is conveniently set up.
	# So whatever one_minute might be, ones[ones_minute] is ones_minute in writing 
	# For example if minute=52 then ones_minute=2 and ones[2]='two'
	ones_minute_writing = ones[ones_minute]

	# Let's get the tens part now. We can do this by simply subtracting the ones_minute from minute
	tens_minute = minute-ones_minute

	# Unfortunately we can't do the same trick we did with ones_minute here to get the word part
	# Because we'll be getting tens[50] if our number is 52. But this can be easily solved
	# We'll just divide tens_minute by 10 to get the number part of it
	# So if tens_minute is 50, we'll get 5 by dividing that and tens[5]='fifty' so it will work out.

	tens_minute_num = tens_minute//10
	# You may find it odd that we use integer division here but the reason is that regular division will return a float
	# And we can't use floats as indices.
	tens_minute_writing = tens[tens_minute_num]

	# Putting it all together
	
	# Now we can simply return hour_writing, tens_minute_writing, ones_minute_writing
	# But our problem here is with the English language. Because 15 is "fifteen" not ten five.
	# We'll first make an if statement to check for this and fix our words accordingly.
	if 10 < minute < 20:
		# Make a list of our 'weird' words
		weird = ['', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
		# This list is also conveniently set up but it just needs a little work.
		# Notice how with our new list, weird[1] = 'eleven'
		# So we can just do weird[ones_minute] to get the word version of a number
		weird_writing = weird[ones_minute]
		#And now we'll return the full word part
		return hour_writing+' '+weird_writing+' '+appendix


	else:
		# Now we'll return the regular. Note that if the minute isn't between 10 and 20 we haven't returned anything yet.
		# return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
		# But we must be aware that if tens_minute_writing or ones_minute_writing is '' then there will be too many spaces
		# We can fix that with a nested if statement 
		if ones_minute_writing == '' and tens_minute_writing == '':
			# This will only work when both of the writings are ''
			return hour_writing+' '+tens_minute_writing+ones_minute_writing+appendix
		elif (ones_minute_writing == '') and (tens_minute_writing != ''):
            # This will work when ones_minute_writing is '' but tens_minute_writing isn't.
			return hour_writing+' '+tens_minute_writing+' '+appendix
		else:
			return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

## Code in full w/o Comments

```python
def time2words(time_string):
	time_list = time_string.split(':')
	hour = int(time_list[0])
	minute = int(time_list[1]) 
    
    ones = ['', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']
	tens = ['', 'ten', 'twenty', 'thirty', 'forty', 'fifty']
    hours = ['twelve', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve']
    
    hour_writing = hours[hour%12]
	if hour == 24:
		appendix = 'am'
	elif hour == 12:
		appendix = 'pm'
	elif hour > 12:
		appendix = 'pm'
	else:
		appendix = 'am'
        
    ones_minute = minute%10
	ones_minute_writing = ones[ones_minute]
    
    tens_minute = minute-ones_minute
    tens_minute_num = tens_minute//10
	tens_minute_writing = tens[tens_minute_num]
	if 10 < minute < 20:
	    weird = ['', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen']
	    weird_writing = weird[ones_minute]
	    return hour_writing+' '+weird_writing+' '+appendix
    else:    
	    if ones_minute_writing == '' and tens_minute_writing == '':
            return hour_writing+' '+appendix
        elif (ones_minute_writing == '') and (tens_minute_writing != ''):
            return hour_writing+' '+tens_minute_writing+' '+appendix
        else:
		    return hour_writing+' '+tens_minute_writing+' '+ones_minute_writing+' '+appendix
```

