---
num: "Lecture 4"
desc: "Shallow vs. Deep Equality, Operator Overloading, Testing / Pytest"
ready: true
lecture_date: 2026-08-11 14:00:00.00-7:00
---

Recorded Lecture: [8_11_26](https://drive.google.com/file/d/1KbAvwBx8nkMsdAghGiuIUCAgDm5ZF-wV/view?usp=drive_link)

# Shallow vs. Deep Equality

* Python allows us to check for equality using the `==` operator for our objects
* But Python doesn’t have any knowledge of what makes a Student equal to another Student in this case
* So by default, Python uses the memory address (not the values) to determine if two objects are the same
* This is known as a **shallow equality**
* Example

```
s1 = Student("Jane", 1234567)
s2 = Student("Jane", 1234567)
print(s1 == s2) # False, doesn’t compare values!
```

* In order to provide our meaning of equality for two Student objects, we will have to define the `__eq__` method in our Student class
* In this case, we can assume two Students are the same if they have the same perm number
	* Comparing values (instead of memory addresses) is called **deep equality**

```
# Add the __eq__ method in Student.py

# s1 == s2, self is the left operand (s1), rhs is the right operand (s2)
def __eq__(self, rhs):
	return self.perm == rhs.perm
```

```
s1 = Student("Jane", 1234567)
s2 = Student("Jane", 1234567)
print(s1 == s2) # True, compares the perm values!
```

# Operator Overloading

* We can define our own behavior for common operators in our classes
	* What does it mean if two student objects are equal (we defined it to mean perm numbers are equal)?
	* Or what does it mean to add (+) two students together?
	* Python allows us to define the functionality for operators!

## Defining `__str__` 

* When printing our defined objects, we may get something unusual. For example:

```
from Student import Student

s1 = Student("Gaucho", 1234567)
s2 = Student("Jane", 5555555)
print(s1) <Student.Student object at 0x7fd5380d8e80>
```

* All objects can be printed, but Python wouldn’t know what to print for user-defined objects like Student
* So it just prints the memory address (the `0x...`) of where the object exists in memory
* In order to provide our own meaning of what Python should display when printing an object like Student, we will need to define a special `__str__` method in our Student class:

```
def __str__(self):
	''' returns a string representation of a student '''
	return "Student name: {}, perm: {}".format(self.name, self.perm) 
```

* Python will now use the return value of the `__str__` method when determining what to display in the print statement
	* Now the `print(s1)` statement outputs `Student name: Gaucho, perm: 1234567`

## Overriding the '+' operator

* What would it mean to add (+) two students together?
* Maybe we can return a list collection ... could be useful ... maybe?

```
def __add__(self, rhs):
	''' Takes two students and returns a list containing these two students '''
    return [self, rhs]
```
```
x = s1 + s2 # returns a list of s1 + s2
print(type(x)) # list type

for i in x:
	print(i)

# Output of for loop
# Student name: Gaucho, perm: 1234567
# Student name: Jane, perm: 5555555
```

## Overloading the '<=' and '>=' operator

* Example:

```
# <=
def __le__(self, rhs):
	''' Takes two students and returns True if the
		student is less than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() <= rhs.name.upper()

# >=
def __ge__(self, rhs):
	''' Takes two students and returns True if the
		student is greater than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() >= rhs.name.upper()

# >
# def __gt__

# <
# def __lt__
```
```
print(s1 <= s2) # True
print(s1 >= s2) # False
print(s1 == s2) # False
print(s1 < s2) # ERROR, we didn’t define the __lt__ method
```

* Good article on this as well as a list of common operators we can overload: <https://thepythonguru.com/python-operator-overloading/>

# Testing

## Complete Test

* Complete Test: Testing every possible path through the code in every possible situation
	* Generally infeasible...
	* Imagine a simple program that takes in 4 integers and prints out the max
		* In Python3, the range of valid integers is a lot!
		* Limited to memory (unlike other languages like C++ / Java where an int type is stored in 32 bits (4 bytes))
		* The number of computations to test EVERY POSSIBLE combination of the 4 integers will take A LONG TIME to compute!

* Unit Testing: Testing individual pieces (units) of a program to ensure correct behavior

## Test Driven Development (TDD)
1. Write test cases that describe what the intended behavior of a unit of software should.
Kinda like the requirements of your piece of software
2. Implement the details of the functionality with the intention of passing the tests
3. Repeat until the tests pass.

* Imagine large software products where dozens of engineers are trying to add new features / implement optimizations all at the same time
* Having a "suite" of tests before deploying software to the public is essential
	* Someone may modify changes that work for a current version, but breaks functionality in a future version
	* Rigorous tests enable confidence in the stability in software

# Pytest
* Pytest is a framework that allows developers to write tests to check the correctness of their code
* Gradescope actually uses pytest to check the "correct" answer with students’ submissions
* We can write functions that start with `test_`, and the body of the function can contain assert statements (as seen in lab00)
	* Pytest will run each of these functions are report which tests passed and which tests failed
* Try and install Pytest on your computer (will use this in our examples)
Installation Guide: <https://docs.pytest.org/en/stable/getting-started.html>
* Windows Installation Guide (created by previous Learning Assistants): [Python and Pytest Installation Guide for Windows](https://drive.google.com/file/d/1nPCwIA8cBAkiJ-kOKZFjkOskD94jmWYn/view)

* Example

Write a function `biggestInt(a,b,c,d)` that takes 4 int values and returns the largest

* Let's write our our tests first (TDD!)

```
# testFile.py

# imports the biggestInt function from lecture.py
from lecture import biggestInt 

def test_biggestInt1():
    assert biggestInt(1,2,3,4) == 4
    assert biggestInt(1,2,4,3) == 4
    assert biggestInt(1,4,2,3) == 4

def test_biggestInt2():
    assert biggestInt(5,5,5,5) == 5
    # etc.

def test_biggestInt3():
    assert biggestInt(-5,-10,-12,-100) == -5
    assert biggestInt(-100, 1, 100, 0) == 100
    # etc.
```

* Now let’s write the function:

```
# lecture.py

def biggestInt(a,b,c,d):
	biggest = 0
	if a >= b and a >= c and a >= d:
		return a
	if b >= a and b >= c and b >= d:
		return b
	if c >= a and c >= b and c >= d:
		return c
	else:
		return d
```

Command to run pytest on testFile.py:
* Navigate to the folder containing `lecture.py` and `testFile.py`
* (On mac terminal): `python3 -m pytest testFile.py`
    * Note: replace `python3` with `python` on Windows.


