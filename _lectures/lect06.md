---
num: "Lecture 6"
desc: "Inheritance cont., Runtime Analysis"
ready: true
lecture_date: 2025-01-23 11:00:00.00-7:00
---

Recorded Lecture: [1_23_25](https://drive.google.com/file/d/18rVUDaQpwV250LKGSqmJ7QP5w1VJ4JTl/view?usp=drive_link)
* Note there were internet difficulties when recording today's lecture, so unfortunately the last 20 minutes were cut off.

# Inheritance and Exceptions

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
