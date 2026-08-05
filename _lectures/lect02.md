---
num: "Lecture 2"
desc: "Python Review cont., Python Errors, Exception Handling"
ready: true
lecture_date: 2026-08-05 14:00:00.00-7:00
---

Recorded Lecture: [8_5_26](https://drive.google.com/file/d/1OG0DS6rr4We2iutmvmR7UJU1szNeKatk/view?usp=drive_link)

# Python Lists

* A dynamic collection of data (heterogeneous types) that can be indexed (index starting at 0).

## Example

```
>>> x = []
>>> len(x)  # returns number of elements in list
>>> x.append(1)	 # adds to the end of the list
>>> x
[1]
>>> len(x)
1
>>> x = [1,2,2,3,3,3]
>>> len(x)
6
>>> x[3]  # extracts an element at an index (index starts at 0)
3
>>> x[3] = "3"  # assigns a value at a certain index
>>> x
[1, 2, 2, '3', 3, 3]
```

* Data types can have methods (functions that can be called on an instance of a type (or object))

```
>>> x = [1,1,2,2,2]
>>> x.count(2)  # counts the number of times 2 appears in the list
3
>>> x.count(3)
0
>>> x = [1,'2',3,'4']
>>> '1' in x  # Returns True if '1' is in the list, False otherwise
False
>>> 1 in x
True
>>> x.insert(2, 2.5)  # inserts 2.5 in index 2 of the list, shifts right elements over
>>> x
[1, '2', 2.5, 3, '4']
>>> x.pop()  # removes and returns last element of list
'4'
>>> x
[1, '2', 2.5, 3]
>>> x.pop(1)  # removes element at index 1
'2'
>>> x
[1, 2.5, 3]
>>> del x[1]  # Notice that there isn’t any output, but still removes element
>>> x
[1, 3]
```

* Python methods and functions may or may not have a return value
* A special value (`None`) is used to represent a non-returned value
* It may be useful to store a return value to a variable, because once a value returns and is not stored, then it’s gone!

# List Slicing

* `[i:j]` syntax starts (includes) element at index i up to (not including) element at index j

## Example

```
>>> x[1:4]
[2, 3, 4]
>>> x[1:7]
[2, 3, 4, 5]
>>> x[1:]
[2, 3, 4, 5]
>>> x[:3]
[1, 2, 3]
```

# Strings

* Strings represent a collection of characters
	* But in python, characters aren’t a data type. It’s basically a string with one value
* Strings are very similar to Lists (not exactly though...)

## Example

```
>>> x = "CS9"
>>> type(x[2])
<class 'str'>
>>> x[2]
'9'
```

But...

```
>>> x[2] = "1"
Traceback (most recent call last):
  File "<pyshell#5>", line 1, in <module>
    x[2] = "1"
TypeError: 'str' object does not support item assignment
```

* Lists in Python are MUTABLE (can change the existing list)
* Strings are IMMUTABLE (cannot change the existing string)

## Some useful string methods

```
>>> x = x.replace("9", "1")  #returns a string,replaces the “9” with “1” in x
>>> x
'CS1'
>>> x.split("S")  # splits the string at the first occurrence of “S”
['C', '1']
>>> x.find("1")  # returns the index of the first occurrence of “1”
2
```

# Functions

```
# Function definition
def double(n):
    ''' Returns 2 times the parameter ''' # Good to comment functions!
    return 2 * n
```

* Note: A function doesn't have to return anything
* If a function doesn't have a specific return value, it returns a special `None` type
* Another note: The function's body's `scope` is defined by tabs

# Mutable vs. Immutable

* Why should we care about immutable vs mutable properties?
	* Depending on whether or not something is mutable or immutable, it affects how the data is treated when passing it in a function
* When immutable types are passed into a function, a COPY of that variable is made and used within the function
	* Once the function returns, the immutable value does not change!
* When mutable types are passed into a function, the parameter is used within the function (no copy is made)
	* Once the function returns, the mutable value does change!

## Example

```
def changeListParameter(x):
    x[0] = "!"
    return x

a = ["C","S","9"]
print(changeListParameter(a))  # ['!', 'S', '9']
print(a)  # ['!', 'S', '9']

def changeStringParameter(x):
    x = x.replace("C", "!")
    return x

b = "CS9"
print(changeStringParameter(b))  # !S9
print(b)  # CS9 - b didn't change!
```

# Control Structures

## if statements

```
if BOOLEAN_EXPRESSION:
	STATEMENT(s)
```

* If BOOLEAN_EXPRESSION is True, execute statements. If False, skip statements
* Note that tabs define the `scope` of the statements as part of the if body.

## if/else statements

```
if BOOLEAN_EXPRESSION:
	STATEMENT(s) #1
else:
	STATEMENT(s) #2
```

* evaluate BOOLEAN_EXPRESSION
	* if True, execute statements #1 and then continue execution after if / else blocks
	* if False, execute statements #2 and then continue execution after if / else blocks

## elif statements

* Same as `else if` statements
* Example

```
x = False
if x:
    print("Shouldn't print")
elif 4 < 5:
    print("4 < 5")
else:
    print("Shouldn't print")
```

## While Loops

```
while BOOLEAN_EXPRESSION:
    STATEMENT(S)
```

* if BOOLEAN_EXPRESSION is True, perform statements in the body of the loop
* if BOOLEAN_EXPRESSION is False, skip the body of loop entirely and continue execution
* Infinite Loop Example:

```
while True:
    print("Weee!!!")  # Will always execute since BOOLEAN_EXPRESSION == True
```

## For loops

```
for VARIABLE in COLLECTION:
	STATEMENT(s)
```

* Assigns an element in the collection to the variable (starts with the 1st item COLLECTION[0])
	* Executes the STATEMENT(s)
* Assigns the next item in the collection to the variable
	* Executes the STATEMENT(s)
* ... and so on…
* Continues loop execution until there are no more items in the collection
* `range()` is a function used for looping if we know the number of iterations we want to make
	* `range(x)` returns a collection of numbers from 0 up to (but not including) x
	* `range(x,y)` returns a collection of numbers starting at x up to (but not including) y
* Example

```
for x in range(4):
	print(x, "Hello!" * x)
	print("---")
```

# Sets

* Like a mathematical set
* A collection of items with:
	* No duplicates
	* Order and position do not matter

## Common set operators

```
s2 = set([2,4,6])		
print(s2)
print(type(s2))
print(3 in s2)
print("?" in s2)
print(5 not in s2) # True
print(len(s2))

# Combine values from two sets
s3 = set([4,5,6])
combined_set = s2 | s3
print(combined_set)

# Get the common elements from two sets
intersecting_set = s2 & s3
print(intersecting_set)
```

# Dictionaries

* Otherwise known as TABLES or MAPS
	* Works where a unique KEY maps to a VALUE
* Gives us more precise indexing than lists

```
DICT = {<key1>:<value1>, <key2>:<value2>, ... , <keyN>:<valueN>}

D = {} # Empty dictionary. Notice the curly braces instead of []
print(D)
print(len(D))

D = {'Jane':18, 'Jen':19, 'Joe':20, 'John':21}

print(D)
print(len(D))
print("Jane's age is:", D['Jane'])

# Simple way to get key / value
for x in D:
    print(x) # Prints the key
    print(D[x]) # Prints the value
```

## Restrictions on using Dictionaries

* Keys must be an immutable type (int, str, namedtuples, tuples, ... not lists for example).
* Values can be any type (immutable or mutable) and do not have to be unique
* For our purposes, KEYS are UNIQUE. Don't define `{'Joe':17, 'Joe':18}`
	* Python is actually OK with duplicate keys in the definition, and it will keep the last key / value in the dictionary

* Example

```
value = D.pop("Joe")  # returns the value associated with Joe, error if doesn’t exist
print(value)
print(D)
value = D.get("jksldf") # returns none if doesn’t exist
print(value)

D["Jem"] = 22 # Adds element to dictionary
print(D)
```

# Python Errors

* We've probably seen Python complain before even running the program
* For example:

```
print("Start")

PYTHON!

print( Hello )
```

* In this case, the program didn’t run at all. Before anything happened
* Python basically is telling us that `PYTHON!` Is a syntax error.
* Before any Python script runs, it gets parsed through and there’s a simple check to make sure all expressions are legal.
* If not, then it will state an error. Note that the program is not running at this time.
* If we remove the syntactically incorrect line:

```
print("Start")
print( Hello )
```

* We get another type of error that happens WHILE the code is executing.
* Errors that happen during program execution is called a runtime error:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( Hello )
NameError: name 'Hello' is not defined
```

* The above message is basically saying `Hello` is a variable that hasn’t been defined, but we’re trying to use it in a print statement.
* Syntactically, there is nothing wrong with the above lines of code (since `Hello` could be a valid variable name).
* In this case, when python tries to execute the statement `print( Hello )`, it throws an exception

# Exceptions

* Exceptions are errors that occur during program execution
* So far, when we’ve encountered these runtime errors, we’ve just noticed that the program crashes
* However, there are ways to recover from runtime errors since we can handle exceptions in our code
* In the above code segment, it’s complaining about a certain type of error called `NameError`
* There are many types of exceptions types that can occur during runtime. For example:

```
print("Start")
print (1/0)
```

* Gives us the exception:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( 1/0 )
ZeroDivisionError: division by zero
```

* In this case, the type of exception that has been thrown is called a `ZeroDivisionError` because a number divided by 0 is undefined

```
print("Start")
print (‘5’ + 5)
```

* Gives us the exception:

```
Traceback (most recent call last):
  File "/Users/richert/Desktop/UCSB/CS9/lecture.py", line 5, in <module>
    print( '5' + 5 )
TypeError: can only concatenate str (not "int") to str
```

* In this case, a `TypeError` occurred since ‘+’ cannot add str and int types together

# Handling Exceptions

The general rule of exception handling is:
* If an exception was raised in a program and nobody catches the exception error, then the program will terminate.
* But we can handle exception messages with a general structure referred to as `try` / `except`
* An example:

```
while True:
	try:
		x = int(input("Enter an int: ")) # input() prompts user for input
		break # breaks out of the current loop
	except Exception:
		print("Input was not a number type")
	print("Let's try again...")

print(x)
print("Resuming execution")
```

The flow of execution is:
* Everything within a `try` block is executed normally.
* If an exception occurs in the `try` block, execution halts and an exception message is passed to a corresponding `except` block.
* The `except` block tries to catch a certain exception type (note that `Exception` catches all types of exceptions (`NameError`, `TypeError`, `ZeroDivisionError`, etc).
* If there is a matching type in the `except` statements, then the first-matching `except` block is executed.
* Once done, program execution resumes past the `except` block(s).
* If no exceptions were caught, then the program will terminate with an error message.