---
num: "Lecture 5"
desc: "Inheritance, Runtime Analysis"
ready: true
lecture_date: 2026-08-12 14:00:00.00-7:00
---

Recorded Lecture: [8_12_26](https://drive.google.com/file/d/1_IM_5qBCM5xCgGI9JbVAD6bX2quFK81W/view?usp=drive_link)

# Inheritance

* Let's write an `Animal` class and see what inheritance looks like in action:

```
# Animal.py
class Animal:
	''' Animal class type that contains attributes for all animals '''

def __init__(self, species=None, name=None):
	self.species = species
	self.name = name

def setName(self, name):
	self.name = name

def setSpecies(self, species):
	self.species = species

def getAttributes(self):
	return "Species: {}, Name: {}".format(self.species, self.name)

def getSound(self):
	return "I'm an Animal!!!"
```

* and now let's define a `Cow` class that inherits from `Animal`

```
# Cow.py

from Animal import Animal

class Cow(Animal):
    # Available method for the Cow Class 
    def setSound(self, sound):
        self.sound = sound
```
```
c = Cow("Cow", "Betsy")
print(c.getAttributes())
c.setSound("Moo") # Sets a Cow sound attribute to "Moo"
print(c.getSound()) # I’m an Animal!!! (calls the Animal.getSound method)
```

* Note that the Cow’s constructor (`__init__`) was inherited from the class `Animal` as well as the `getAttributes()` method
* Also note that we didn’t need to define the `getSound()` method since it was inherited from `Animal`
* But in this case, this inherited method `getSound()` may not be what we want.
* So we can redefine its functionality in the Cow class!


```
# in Cow class
def getSound(self):
	return "{}!".format(self.sound)
```

* We changed the `getSound()` method in the `Cow` class, so in this case our `Cow` class overrode the `getSound()` method of `Animal`
* So now, cow objects will use its own version of `getSound()`, not the version that was inherited from `Animal`, as seen below:

```
c = Cow("Cow", "Betsy")
c.setSound("Moo") # Sets a Cow sound to "Moo"
print(c.getSound()) # Moo!
```

* We can still create `Animal` objects, and `Animal` objects will still use its own version of `getSound()`

```
a = Animal("Unicorn", "Lala")
print(a.getAttributes())
print(a.getSound()) # I’m an Animal!!!
```

<b>Note:</b> The constructed object type will dictate which method in which class is called.
* It first looks at the <b>constructed object type</b> and checks if there is a method defined in that class. If so, it uses that
* If the constructed object doesn’t have a method definition in its class, then it checks the parent(s) it inherited from, and so on ...
* If there is no matching method call, then an error happens

## Extending Superclass Methods

* Some terminology:

* `Animal` in the previous example can be referred to as the <b>Base / Parent / Super class</b>
* `Cow` can be referred to as the <b>Sub / Child / Derived class</b>
* In the example above, we overrode the `makeSound` method from `Animal`
* However, sometimes we only want to extend the functionality, not completely replace it.
	* It is possible to override methods and still use the inherited functionality by calling the `super()` class methods
	* So in this case, we override the method in the child class, but we extend the base class’ functionality by using it in the child class’ overwritten method
	* Example:

```
class Cow(Animal):
	def getSound(self):
		s = "Using Super class getSound method\n"
		s += Animal.getSound(self) + "\n" # Uses Animal.getSound method
		s += "Extending it with our own getSound functionality" + "\n"
		s += "{}!!!".format(self.sound, self.sound)
		return s

# Output:
# Using super class getSound method
# I'm an Animal!!!
# Extending it with our own getSound functionality
# Moo!!!
```

## Extending Constructors in a Child Class

* A common pattern is to redefine a subclass’ constructor by taking in all data from its parent class AND data specific to the subclass.
* Example:

```
# In Cow.py

    def __init__(self, species=None, name=None, sound=None):
		super().__init__(species, name)
		#Animal.__init__(self, species, name) also works
		self.sound = sound
```
```
c = Cow("Cow", "Betsy", "Moo") # Passes in data for Animal AND Cow
a = Animal("Unicorn", "Lala")

zoo = [c, a]

for i in zoo:
	print(i.getAttributes())
	print(i.getSound())
	print("---")
```

## Inheritance and Exceptions

* We can create a hierarchy of Exception types using inheritance
	* Exception is the base class for ALL Exception types (Python allows us to raise these types)
	* Remember the sub-class **IS-A** type of the base class
	* This may be useful for fine-tuning certain behavior
	* For example, a network error could be because of:    
		* Malformed URL
		* Timeout
		* ...
	* Or file reading may error out due to:
		* Incorrect file name
		* Incorrect access permissions
		* ...
* Example of creating our own Exception types:

```
class A(Exception):
	pass

class B(A): # B inherits from A (B IS-A A type)
	pass

class C(Exception):
	pass

try:
	x = int(input("Enter a positive number: "))
	if x < 0:
		raise B() # Change this to A() and C() and observe...
except C:
	print("Exception of type C caught")
except A:
	print("Exception of type A caught")
except B:
	print("Exception of type B caught") # Will never get called
except Exception:
	print("Exception of type Exception caught")

print("Resuming execution")
```

* In the example above, B's except block is never called.
	* This is because B **IS-A** type of A (B is a child of A). So when we catch the matching type, A always matches first.

# Algorithm Analysis

* There are many ways we can try to estimate an algorithm
* For example, we can benchmark the algorithm by calculating the time it takes for something to run
* We can do this in Python using some code:

```
import time

def f1(n):
	l = []
	for i in range(n):
		l.insert(0,i)
	return

def f2(n):
	l = []
	for i in range(n):
		l.append(i)
	return

print("starting f1")
start = time.time()
f1(200000)
end = time.time()
print("time elapsed: ", end - start, "seconds")

print("starting f2")
start = time.time()
f2(200000)
end = time.time()
print("time elapsed: ", end - start, "seconds")
```

* We’ll get to why the time difference between adding to the list in the beginning and adding to the end differ in time soon enough :)
* This way of measuring algorithms has some problems like:
	* Underlying hardware (fast / slow CPU, amount of memory, disk size / speed, network speed, etc.)
	* How busy the computer is, how the OS is managing other programs
	* How big n is (if n is small, time is almost the same)

## Asymptotic Behavior

* We want to analyze approximately how fast an algorithm runs when the size of the input approaches infinite
* So instead of calculating the raw time of how fast the algorithm runs on our computers, we can approximate the number of instructions the algorithm will take with respect to the size of the input
	* Steps can include things like assigning values to variables, evaluating boolean expressions, arithmetic operations, etc.
* Let’s try this with a simple algorithm:

```
for i in range(10):
	print(i)
```

* Counting expressions in this case:
	* Assignment: i = 0, i = 1, i = 2, etc. (10 steps)
	* print() (10 steps)
	* Algorithm takes 20 steps

* The algorithm runs in **constant time**, since there isn’t a variable input and will always take the same number of instructions to run.
* Let’s change our problem taking in a variable input size:

```
def f(n):
	for i in range(n):
		print(i)
```

* Now the number of instructions in this algorithm is dependent on the value of n
* So let’s try to express the number of instructions as a polynomial with respect to n
	* We can denote the number of instructions with respect to n as T(n)
* T(n) = n assignment statements + n print statements
* T(n) = 2n

## Order of magnitude function (Big-O)
* Since we have no idea how large n will be, we want to assume the **worst-case** scenario when analyzing our algorithms
	* And in this case, the worst-case is when n approaches infinite
* Since n approaches infinite, we can ignore lower order terms and coefficients
	* Does the 3 really matter when we have 1,000,000,000,000,000,000 + 3 ?
	* See: <http://science.slc.edu/~jmarshall/courses/2002/spring/cs50/BigO/index.html> for an example of how lower-order terms converge when n approaches infinite
* So we can express the above algorithm above by dropping all lower order terms, constants, and coefficients to **O(n)**
* **Note:** constant time algorithms are denoted as **O(1)**
* Another (slightly) more complex example:

```
def f(n)
	x = 0
	for i in range(n):
		for j in range(n):
			x = i + j
```

* Initialize x = 0 is 1 instruction
* i assignment statements (n instructions)
* j assignment statements (n<sup>2</sup> instructions)
* i + j computations (n<sup>2</sup> instructions)
* x reassignment statements (n<sup>2</sup> instructions)
	* T(n) = 1 + 3n<sup>2</sup> + n
* Drop all lower order terms, coefficients, and constants and we get **O(n<sup>2</sup>)**
* Another example:

```
def f(n):
    for i in range(n):
        return i
```

* Note that in this example, i gets assigned to 0.
* But then i is immediately returned before re-assigning i to 1.
* So this algorithm is **O(1)**.
